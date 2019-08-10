//importing
import java.applet.Applet;
import java.applet.AudioClip;
import java.awt.*;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import java.awt.event.MouseEvent;
import java.awt.event.MouseListener;
import java.io.*;
import java.net.MalformedURLException;
import java.net.URL;
import java.util.Arrays;
import javax.swing.*;
import java.awt.event.*;

public class Brainstorm extends JPanel implements ActionListener, MouseListener, KeyListener
{
	static JFrame frame;
	final int SQUARE_SIZE = 60;
	final int TOP_OFFSET = 42;
	final int BORDER_SIZE = 4;

	//setting up global variables
	int[] [] board;
	int[] [] board2;
	int[] [] index;
	int levels = 1;;
	int currentPlayer;
	int currentColumn;
	Timer timer;
	int time;
	int music = 1;
	Image firstImage;
	Image coverImage;
	Image offScreenImage;
	Graphics offScreenBuffer;
	@SuppressWarnings("deprecation")
	AudioClip backGroundSound;
	@SuppressWarnings("deprecation")
	AudioClip beep;
	
	boolean gameOver;
	int cover = 0;
	int remove = 15;
	int[] beginnerScoreBoard = new int[3];
	int [] intermediateScoreBoard = new int[3];
	int[] advancedScoreBoard = new int[3];

	int chosenN = 0; //chosen number
	int chosenR = 0; //chosen row
	int chosenC = 0; //chosen Column

	JLabel title;


	//buttons
	Image [] nImages;
	Image [] cropped = new Image[10];
	
	//a method to get the sound in to the game
	public URL getCompleteURL (String fileName)
	{
		try
		{
			return new URL ("file:" + System.getProperty ("user.dir") + "/" + fileName);
		}
		catch (MalformedURLException e)
		{
			System.err.println (e.getMessage ());
		}
		return null;
	}


	public Brainstorm() {

		//setting up the background settings
		setLayout(null);

		setPreferredSize (new Dimension (12 * SQUARE_SIZE + 2 * BORDER_SIZE + 1, (10 + 1) * SQUARE_SIZE + TOP_OFFSET + BORDER_SIZE + 1));
		setLocation (100, 50);

		frame.setBackground (new Color (150, 50, 70));
		setLayout (new BoxLayout (this, BoxLayout.PAGE_AXIS));

		board = new int [11] [11];

		frame.setResizable(false);
		time = 0;

		timer = new Timer (100, new TimerEventHandler ());


		//		try {
		//		    //create the font to use. Specify the size!
		//		    Font font1 = Font.createFont(Font.TRUETYPE_FONT, new File("font.ttf")).deriveFont(12f);
		//		    GraphicsEnvironment ge = GraphicsEnvironment.getLocalGraphicsEnvironment();
		//		    //register the font
		//		    ge.registerFont(font1);
		//		} catch (IOException e) {
		//		    e.printStackTrace();
		//		} catch(FontFormatException e) {
		//		    e.printStackTrace();
		//		}


		title = new JLabel ("Sudoku");
		title.setForeground(Color.black);
		//		title.setFont(font1);
		//		title.setBounds (x: 300, y:300, height: 50, width:100);

		//set up the music
		backGroundSound = Applet.newAudioClip (getCompleteURL ("loop.wav"));
		backGroundSound.loop();
		beep = Applet.newAudioClip (getCompleteURL ("beep.wav"));

		// Set up the Menu
		// Set up the Game MenuItems
		JMenuItem newOption, exitOption;
		newOption = new JMenuItem ("New");
		exitOption = new JMenuItem ("Exit");
		JMenuItem help = new JMenuItem("Help");
		JMenuItem score = new JMenuItem("Scoreboard");
		JMenuItem beginnerOption = new JMenuItem("Beginner");
		JMenuItem intermediateOption = new JMenuItem("Intermediate");
		JMenuItem advancedOption = new JMenuItem("Advanced");
		JMenuItem musicOption = new JMenuItem("Mute");
		JMenuItem playOption = new JMenuItem("Play");

		// Set up the Game Menu
		JMenu gameMenu = new JMenu ("Game");
		// Add each MenuItem to the Game Menu (with a separator)
		gameMenu.add (newOption);
		gameMenu.addSeparator ();
		gameMenu.add (help);
		gameMenu.addSeparator ();
		gameMenu.add (score);
		gameMenu.addSeparator ();
		gameMenu.add (exitOption);

		//set up the Levels menu
		JMenu difficultyMenu = new JMenu("Levels");
		difficultyMenu.add(beginnerOption);
		difficultyMenu.addSeparator();
		difficultyMenu.add(intermediateOption);
		difficultyMenu.addSeparator();
		difficultyMenu.add(advancedOption);
		
		//set up the Music menu
		JMenu musicMenu = new JMenu("Music");
		musicMenu.add(musicOption);
		musicMenu.addSeparator();
		musicMenu.add(playOption);
		
		//add all the menus
		JMenuBar mainMenu = new JMenuBar ();
		mainMenu.add (gameMenu);
		mainMenu.add(difficultyMenu);
		mainMenu.add(musicMenu);

		// Set the menu bar for this frame to mainMenu
		frame.setJMenuBar (mainMenu);

		//the number buttons
		//Use a media tracker to make sure all of the images are
		//loaded before we continue with the program

		nImages = new Image [9];

		MediaTracker tracker = new MediaTracker (this);
		for (int i = 0 ; i < 9; i++) {
			nImages [i] = Toolkit.getDefaultToolkit ().getImage(Integer.toString(i + 1) + ".png");
			tracker.addImage (nImages [i], i);
		}

		//Images
		MediaTracker tracker2 = new MediaTracker (this);
		firstImage = Toolkit.getDefaultToolkit ().getImage ("image2.png");
		tracker2.addImage (firstImage, 0);
		coverImage = Toolkit.getDefaultToolkit ().getImage ("image1.png");
		tracker2.addImage (coverImage, 0);

		//New game and exit game options
		newOption.setActionCommand ("New");
		newOption.addActionListener (this);
		help.setActionCommand ("Help");
		help.addActionListener (this);
		score.setActionCommand ("Scoreboard");
		score.addActionListener (this);
		exitOption.setActionCommand ("Exit");
		exitOption.addActionListener (this);

		//difficulty levels
		beginnerOption.setActionCommand("Beginner");
		beginnerOption.addActionListener(this);
		intermediateOption.setActionCommand("Intermediate");
		intermediateOption.addActionListener(this);
		advancedOption.setActionCommand("Advanced");
		advancedOption.addActionListener(this);

		//background music
		musicOption.setActionCommand("Mute");
		musicOption.addActionListener(this);
		playOption.setActionCommand("Play");
		playOption.addActionListener(this);

		setFocusable (true); // Need this to set the focus to the panel in order to add the keyListener
		addKeyListener (this);
		addMouseListener (this);

	} // Constructor

	private class TimerEventHandler implements ActionListener
	{
		// The following method is called when the user plays a game. The timer stops when 
		public void actionPerformed (ActionEvent event)
		{
			if (gameOver) {
				timer.stop();
			}
			else
			{
				// Increment the time 
				time++;
				if (time % 10 == 0) 
					repaint ();

			}
		}

	}
	//setting up what each menu would do
	public void actionPerformed (ActionEvent event)
	{
		String eventName = event.getActionCommand ();
		if (eventName.equals ("New"))
		{
			cover = 1;
			newGame ();
		}
		else if (eventName.equals("Help")){
			cover = 2;
			repaint();
		}
		else if (eventName.equals("Scoreboard")) {
			cover = 3;
			repaint();
		}
		else if (eventName.equals ("Exit"))
		{
			System.exit (0);
		}
		else if (eventName.equals("Beginner")) {
			remove = 1;
			levels = 15;
			newGame();
			repaint();
		}
		else if (eventName.equals("Intermediate")) {
			remove = 25;
			levels = 25;
			newGame();
			repaint();
		}
		else if (eventName.equals("Advanced")) 
		{
			levels = 3;
			remove = 35;
			newGame();
			repaint();
		}
		else if (eventName.contentEquals("Mute"))
		{
			music = 0;
			backGroundSound.stop();
		}
		else if (eventName.equals("Play"))
		{
			music = 1;
			backGroundSound.loop();
		}
	}

	//This method creates a new game when the current game is finished or if the user wants to start a new game. First, it clears the board using clearBoard() method. 
//	Next, the three diagonal 3x3 matrices are filled using diagonalBox() method, By using fillRest() method, the rest of the grid gets filled. 
//	All of this was done to create a new random grid for the new puzzle. This doesn’t have any parameters or return anything.
	public void newGame ()
	{	
		cover = 1;
		time = 0;
		timer.restart();
		clearBoard ();
		diagonalBox(1,1);
		diagonalBox(4,4);
		diagonalBox(7,7);
		fillRest(1,1);
		if (board[9][6] == 0)
			newGame();
		removeNum(remove);

		gameOver = false; //???? do we need this ???? idgi how does it work
		repaint ();
	}
	
	//compare baord (the solution) and board2 (the puzzle that the user sees) to see if the user completed the puzzle correctly.
//This doesn't have any parameters or return value. If the user completed the puzzle correctly, the gameOver variable is set to true. Else, false.
	public void compareBoard() {
		int count = 0;
		for(int i = 0; i < 11; i++) {
			for (int j = 0; j < 11 ; j++) {
				if (board[i][j] == board2[i][j])
					count++;
				else {
					gameOver = false;
					return;
				}

			}

		}
		if (count >= 121) 
			gameOver = true;
		return;

	}
	
	//compare the time the user took to figure out the top 3 scores
	//The parameter is the scoreboard of the difficulty level the user has played. 
	//It returns void. It changes the values in the scoreBoard. 

	public void compareTime(int[] scoreBoard) {

		int score = (int)(time/10);
		int index = -1;
		for (int i = 0; i <3 ; i++) {
			if (scoreBoard[i] > score && scoreBoard[i] != 0)
				index ++;
			else if (scoreBoard[i] == 0) {
				index ++;
			}
			else if(score == scoreBoard[i]) {
				index = -1;
				return;
			}
				

		}
		if(index == 2) {
			scoreBoard[0] = scoreBoard[1];
			scoreBoard[1] = scoreBoard[2];
			scoreBoard[2] = score;
			
		}
		else if (index == 1) {
			scoreBoard[0] = scoreBoard[1];
			scoreBoard[1] = score;
		}
		else if(index == 0)
			scoreBoard[index] = score;

	}

//	The purpose of this method is to clear the used board in order to generate a new board. There are no parameters or return value.
//	It changes all the numbers in the grid to 0.
	public void clearBoard ()
	{

		for(int a = 0; a<=board.length-1; a++) {
			for(int b = 0; b<= board[a].length-1; b++) {
				board[a][b] = 0;
			}
		}
	}

//	There are two parameters: currentColumn, and num. currentColumn represents the column that computer is at, and num represents the random number that needs to be checked. 
//	This method checks if there is a number with the same value as num in the same row. If there is, it will return false, and if there is none, it will return true.
//	Using a for loop, it will go through all the numbers in the same row and check if the numbers are the same as num. Since the column is not being changed, its value remain constant, as a variable currentColumn.
	public boolean checkRow (int currentColumn, int num) {
		for (int i = 1; i<=9; i++) {
			if (board[i][currentColumn] == num)
				return false;
		}

		return true;
	}

//	There are two parameters: currentRow, and num. currentRow represents the row that computer is at, and num represents the random number that needs to be checked.
//	This method checks if there is a number with the same value as num in the same column. If there is, it will return false, and if there is none, it will return true. 
//	Using a for loop, it will go through all the number in the same column and check if the numbers are the same as num. Since the row is not being changed, its value remain constant, as a variable currentRow.
	public boolean checkColumn (int currentRow, int num) {
		for (int i =1; i <= 9; i++) {
			if (board[currentRow][i] == num)
				return false;
		}

		return true;
	}

//	The purpose of this method is to check if there is a number with the same value as num in the same 3 x 3 box
//	There are three parameters: currentColumn, currentRow, and num. currentRow represents the row that computer is at, currentColumn represents the column that computer is at, and num represents the random number that needs to be checked.
// If there is a same number as num in the box, it will return false. If there is none, it will return true.
	public boolean checkBox (int currentColumn, int currentRow, int num) {		
		int column = currentColumn % 3;
		if (column == 1)
			column = currentColumn;
		else if (column == 0)
			column = currentColumn - 2;
		else 
			column = currentColumn -1;

		int row = currentRow %3;
		if (row == 0)
			row = currentRow - 2;
		else if (row == 2)
			row = currentRow -1;
		else if (row == 1)
			row = currentRow;

		if (column >=10 || row >= 10)
			return false;

		for (int i = 0; i < 3; i ++)
			for(int j = 0; j < 3; j++)
				if(board[row+i][column+j] == num) {
					return false;
				}


		return true;
	}

//	This method generates a complete 3x3 matrix with numbers from 1-9 randomly generated using generateArray(). This has two parameters: row, and column. 
//	Row represents the starting row of the box, and column represents the starting column of the box. This is used in the program to make the algorithm faster and easier.
	public void diagonalBox (int row, int column) {
		int[] list = generateArray();
		int count = 0;
		for (int r = 0; r<3; r++) {
			for (int c = 0; c < 3; c++) {
				//				for (int i = 0; i < 9; i++) {
				board[row+r][column+c] = list[count];
				count++;
				//				}
			}
		}	
	}

//	This method fills in the rest of the empty cells by recursively checking a random number (must be from 1-9) if it fits in the cell.
//	If it does, then it puts the number in the cell. If it doesn’t, it tries other numbers until there is no option but to backtrack.
//	The checking is done using checkRow, checkColumn, and checkBox. This has no parameters, or any return value. This method is necessary to generate the rest of the grid
	public void fillRest (int row, int column ) {
		if (column == 7 && row == 10) {
			return;
		}

		if (board[row][column] == 0) {
			for (int i = 1; i <=9; i++) {
				if( checkRow(column, i) == true && checkColumn(row, i) == true && checkBox(column, row, i) == true) {
					board[row][column] = i;
					if(column == 9)
						fillRest(row + 1, 1);
					else { 
						fillRest(row, column +1);
					}
				}
			}

		}
		else {
			if(column == 9)
				fillRest(row + 1, 1);
			else {
				fillRest(row, column +1);
			}
		}

	}
//	This method generates and returns an integer array with numbers from 1-9 randomly sorted. This doesn’t have any parameters.
	public int[] generateArray() {
		int[] numbers = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};
		int[] reappear = new int [9];
		int count = 0;
		for (int i = 0; i < 9; i++) {
			while(reappear[i] == 0) {
				int random = (int)(Math.random()*(9)+1);
				for (int j = 0; j< 9; j++) {
					if (numbers[random] == reappear[j])
						count++;
				}
				if (count == 0)
					reappear[i] = numbers[random];
				count = 0;
			}
		}
		return reappear;
	}

//	This method removes a number of cells in the grid to create a puzzle. This resultant grid is the grid that the user will see. 
//	There is one parameter, remove, which is the number of cells being removed.
//	The number 'remove' will depend on the difficulty level the user chooses. 
	public void removeNum(int remove) {
		index = new int [35][2];
		board2 = new int [11][11];
		for (int i = 0; i <11 ; i++) {
			for (int j = 0; j < 11; j ++) {
				board2[i][j] = board[i][j];
			}
		}

		for (int i = 0; i < remove; i++) {
			int random = (int)(Math.random()*(9)+1);
			int random2 = (int)(Math.random()*(9)+1);
			index[i][0] = random;
			index[i][1] = random2;
			board2[random][random2] = 0;
		}
	}
	//this method uses compareTime() method to check the score of the user when the game is finished.
	//If the user gets it right, a message will be shown congratulating the user. 
	//This method returns void, and there is no parameter. 
	public void handleAction ()
	{
		compareBoard();
		if (gameOver == true)
		{
			if (levels == 1)
				compareTime(beginnerScoreBoard);
			else if (levels == 2)
				compareTime(intermediateScoreBoard);
			else if (levels == 3)
				compareTime(advancedScoreBoard);

			JOptionPane.showMessageDialog (this, "Congrats! You solved the puzzle! Please select 'new' to play another game.",
					"Game Over. CONGRATS", JOptionPane.WARNING_MESSAGE);
			return;
		}
		return;
	}


	public void paintComponent (Graphics g)
	{
		// Set up the offscreen buffer the first time paint() is called
		if (offScreenBuffer == null)
		{
			offScreenImage = createImage (this.getWidth (), this.getHeight ());
			offScreenBuffer = offScreenImage.getGraphics ();
		}
		Font customFont=null;
		try {
			//create the font to use. Specify the size!
			customFont = Font.createFont(Font.TRUETYPE_FONT, new File("font.ttf")).deriveFont(12f);
			// GraphicsEnvironment ge = GraphicsEnvironment.getLocalGraphicsEnvironment();
			//register the font
			// ge.registerFont(customFont);
		} catch (IOException e) {
			e.printStackTrace();
		} catch(FontFormatException e) {
			e.printStackTrace();
		}

		Font customFont1 = null;
		try {
			//create the font to use. Specify the size!
			customFont1 = Font.createFont(Font.TRUETYPE_FONT, new File("font2.ttf")).deriveFont(12f);
			// GraphicsEnvironment ge = GraphicsEnvironment.getLocalGraphicsEnvironment();
			//register the font
			// ge.registerFont(customFont);
		} catch (IOException e) {
			e.printStackTrace();
		} catch(FontFormatException e) {
			e.printStackTrace();
		}

		Font titleF = customFont.deriveFont (customFont.getSize () * 3.5F);
		Font content = customFont.deriveFont (customFont.getSize () * 2F);
		Font smallTitle = customFont.deriveFont (customFont.getSize () * 2.5F);
		Font rules = customFont1.deriveFont (customFont1.getSize () * 1.8F);
		Font lines = customFont.deriveFont(customFont.getSize()*3F);


//		This is the title page that shows the title, play button, help button, and scoreboard button.
		if (cover == 0) {
			offScreenBuffer.setFont(titleF);
			offScreenBuffer.drawImage(firstImage, 0, 0, 760, 760, this);
			offScreenBuffer.setColor(Color.white);
			offScreenBuffer.fillRect(170, 150, 370, 90);
			offScreenBuffer.fillRect(230, 340, 250, 50);
			offScreenBuffer.fillRect(230, 420, 250, 50);
			offScreenBuffer.fillRect(230, 500, 250, 50);
			offScreenBuffer.setColor(Color.black);
			offScreenBuffer.drawString("Sudoku", 288, 220);
			offScreenBuffer.setFont(content);
			offScreenBuffer.drawString("play", 330, 375);
			offScreenBuffer.drawString("help", 330, 460);
			offScreenBuffer.drawString("scoreboard", 300, 540);

		}

		// All of the drawing is done to an off screen buffer which is
		// then copied to the screen.  This will prevent flickering
		// Clear the offScreenBuffer first

//		this is to show the puzzle on jframe. 
		if (cover == 1) {
			offScreenBuffer.clearRect (0, 0, this.getWidth (), this.getHeight ());
			offScreenBuffer.drawImage(coverImage, 0, 0, 800, 800, this);
			
			// Redraw the board with current pieces
			for (int row = 1 ; row <= 9 ; row++)
				for (int column = 1 ; column <= 9 ; column++)
				{
					// Find the x and y positions for each row and column
					int xPos = (column - 1) * SQUARE_SIZE + BORDER_SIZE;
					int yPos = TOP_OFFSET + row * SQUARE_SIZE;
					
					// Draw the squares
					offScreenBuffer.setColor (Color.white);
					offScreenBuffer.fillRect (xPos, yPos, SQUARE_SIZE, SQUARE_SIZE);
					offScreenBuffer.setColor (Color.black);
					offScreenBuffer.drawRect (xPos, yPos, SQUARE_SIZE, SQUARE_SIZE);
					
					//Draw the numbers 

					if(board2[row][column] == 0) {
						offScreenBuffer.setColor (Color.white);
						}
					else 
						offScreenBuffer.setColor (Color.gray);

					for (int i = 0; i < 35; i ++)
						if (row == index[i][0] && column == index [i][1] && board2[row][column] != 0)
							offScreenBuffer.setColor (Color.BLACK);

					offScreenBuffer.drawString(Integer.toString(board2[row][column]), xPos+ (SQUARE_SIZE/2), yPos+SQUARE_SIZE/2);

					MediaTracker tracker3 = new MediaTracker (this);
					for (int i = 0 ; i < 9; i++) {
						cropped [i] = Toolkit.getDefaultToolkit ().getImage(Integer.toString(i+1) + "c.png");
						tracker3.addImage (cropped [i], i);
					}
					
					//Draw the number buttons
					//Images [] images = {n1, "n2", "n3", "n4", "n5", "n6", "n7", "n8", "n9", "n10"};
					int a = 100;
					for(int i = 0; i <= 8; i++) {
						offScreenBuffer.drawImage (nImages[i], 600, a, this);
						a += 60;
					}
					
					//Bring the wooden number image if a number is chosen, in order to make it more obvious for the user that
					//which number they just picked
					if(chosenN != 0) {
						offScreenBuffer.drawImage(cropped[chosenN-1], 600, 100+(chosenN-1)*60, this);
					}
					
					//Draw the 9 
					offScreenBuffer.setColor (Color.black);
					offScreenBuffer.setFont(lines);
					offScreenBuffer.drawLine (185,105,185,645);
					offScreenBuffer.drawLine (365,105,365,645);
					offScreenBuffer.drawLine (5, 283, 545, 283);
					offScreenBuffer.drawLine (5, 463, 545,463);
					
				
					
					offScreenBuffer.setFont(content);
					offScreenBuffer.setColor (Color.black);
					offScreenBuffer.drawString ("Time:   " + ((int)(time/10.0) ), 100, 100);
					
					offScreenBuffer.setColor(Color.black);
					offScreenBuffer.drawString("Clear", 580, 680);

				}
		}
		//This is the help page which shows the users the rules of this game.
		else if(cover == 2) {
			offScreenBuffer.clearRect (0, 0, this.getWidth (), this.getHeight ());
			offScreenBuffer.drawImage(coverImage, 0, 0, 800, 800, this);
			offScreenBuffer.setColor (Color.white);
			offScreenBuffer.fillRect(70, 70, 550, 550);
			offScreenBuffer.setColor (Color.black);
			offScreenBuffer.setFont(smallTitle);
			offScreenBuffer.drawString("Rules:", 130,130);
			offScreenBuffer.setFont(rules);
			offScreenBuffer.drawString("Each number can only appear once in a row or a column", 120,170);
			offScreenBuffer.drawString("or a box", 120,200);
			offScreenBuffer.drawString("When you complete the puzzle, click the board once", 120,230);
			offScreenBuffer.drawString("A pop up box will congrat you if you solve the puzzle", 120,260);
			offScreenBuffer.drawString("Nothing will happen if the puzzle is wrong", 120,290);
			offScreenBuffer.drawString("If you wish to remove a number you just put down", 120, 320);
			offScreenBuffer.drawString("Click the Clear button on the right", 120, 350);
			offScreenBuffer.drawString("If you wish to remove a number you put down previously", 120, 380);
			offScreenBuffer.drawString("Click the box you wish to clear and click clear button", 120, 410);
			offScreenBuffer.drawString("Different difficulty levels can be chosen in the menu", 120,440);
			offScreenBuffer.drawString("as well as the option to play music", 120,470);
			offScreenBuffer.drawString("HAVE FUN !", 120,520);
			offScreenBuffer.setColor (Color.RED);
			offScreenBuffer.fillRect(430, 500, 150, 50);
			offScreenBuffer.setColor (Color.WHITE);
			offScreenBuffer.setFont(smallTitle);
			offScreenBuffer.drawString("play", 465,535);

		}

		
		//This is the scoreboard page. The top 3 score from each level are selected to be shown.
		else if (cover == 3) {
			offScreenBuffer.clearRect (0, 0, this.getWidth (), this.getHeight ());
			offScreenBuffer.drawImage(coverImage, 0, 0, 800, 800, this);
			offScreenBuffer.setColor (Color.white);
			offScreenBuffer.fillRect(70, 70, 550, 550);
			offScreenBuffer.setColor (Color.green);
			offScreenBuffer.setFont(titleF);
			offScreenBuffer.drawString("Scoreboard", 130, 130);	
			offScreenBuffer.setFont(content);
			offScreenBuffer.setColor (Color.gray);
			offScreenBuffer.drawString("Beginner", 130, 190);
			for (int i = 0; i < 3; i++) {
				int leader = beginnerScoreBoard[2-i];
				offScreenBuffer.drawString(i+ 1 + ".  "+ leader + " secs", 150, 240 + 50 * i);
			}
			offScreenBuffer.drawString("Intermediate", 130, 400);

			for (int i = 0; i < 3; i++) {
				int leader = intermediateScoreBoard[2-i];
				offScreenBuffer.drawString(i+ 1 + ".  "+ leader + " secs", 150, 450 + 50 * i);
			}
			offScreenBuffer.drawString("Advanced", 330, 190);

			for (int i = 0; i < 3; i++) {
				int leader = advancedScoreBoard[2-i];
				offScreenBuffer.drawString(i+ 1 + ".  "+ leader + " secs", 330, 240 + 50 * i);
			}

		}

		g.drawImage (offScreenImage, 0, 0, this);

	}

	public void keyTyped(KeyEvent e) {
		// TODO Auto-generated method stub

	}

	public void keyPressed(KeyEvent e) {
		// TODO Auto-generated method stub

	}

	public void keyReleased(KeyEvent e) {
		// TODO Auto-generated method stub

	}

	public void mouseClicked(MouseEvent e) {
		// TODO Auto-generated method stub

	}

	public void mousePressed(MouseEvent e) {
		// TODO Auto-generated method stub

		Point point = new Point (e.getX (), e.getY ());
		int x = e.getX();
		int y = e.getY();
		Point selectedPoint = new Point (e.getX (), e.getY ());
		//		System.out.println(x +      "     " + y);
		// - infoPanel.getHeight ()); // must minus the height of the panel above
		
		//If the user didn't click mute, there is a sound effect whenever the user clicks something.
		if (music != 0) {
			beep.play();

		}

		//On the title page, there are three buttons that are created here. 
		if(cover == 0) {
			if(x>230 && x<480) {
				if(y>342 && y<392){
					cover = 1;

					newGame();
				}
				if(y>422 && y<472) {
					cover = 2; //the help page
					//System.out.println("Hello");
					repaint();
				}
				if(y>502&& y<552){
					cover = 3; //the score board
					repaint();
				}
			}
		}
		
		//these are the buttons for numbers on the game page. It only changes the numbers on the cells 
//		that users can change.
		if(cover == 1) {
			handleAction();

			if(x>590 && x<650 && y>100 && y<640) {
				chosenN = (y-100)/60+1;	
				Image pic;
				MediaTracker tracker = new MediaTracker (this);
					pic = Toolkit.getDefaultToolkit ().getImage(chosenN+ "c.png");
					tracker.addImage (pic,1);
				}
			

			if (x>5 && x<545 && y>105 && y<645) {
				chosenC = (x-5)/60+1;
				chosenR = (y-105)/60+1;

				for (int i = 0; i <35 ; i ++) {
					if(chosenR == index[i][0] && chosenC == index[i][1]) {
						board2[chosenR][chosenC] = chosenN;
						repaint();
						break;
					}
				}

			}
			if(x>547 && x<652&& y>650 && y<690){
				board2[chosenR][chosenC] = 0;
			}
		}
		//this is for the play button on the help page. 
		if(cover == 2) {
			if(x>430 && x<580 && y>500 & y<555){
				newGame();
				repaint();
			}

		}

	}

	public void mouseReleased(MouseEvent e) {
		// TODO Auto-generated method stub

	}

	public void mouseEntered(MouseEvent e) {
		// TODO Auto-generated method stub

	}

	public void mouseExited(MouseEvent e) {
		// TODO Auto-generated method stub

	}

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		//setting up jframe
		
		frame = new JFrame ("Sudoku");
		Brainstorm myPanel = new Brainstorm ();

		frame.add (myPanel);
		frame.pack ();
		frame.setVisible (true);
	}
}
