import org.eclipse.swt.widgets.Display;
import org.eclipse.swt.widgets.Shell;
import org.eclipse.swt.widgets.Canvas;
import org.eclipse.swt.SWT;
import org.eclipse.swt.graphics.GC;
import org.eclipse.swt.widgets.Label;
import org.eclipse.swt.widgets.Listener;
import org.eclipse.swt.widgets.MessageBox;
import org.eclipse.swt.widgets.Button;
import org.eclipse.swt.widgets.Text;


public class Hangman {

	protected Shell shlHangmanGame;
	private Text text;
	
	WordFactory myWords = new WordFactory();
	HangmanDisplay myDisplay = new HangmanDisplay();
	Man myMan = new Man();

	/**
	 * Launch the application.
	 * @param args
	 */
	public static void main(String[] args) {
		try {
			Hangman window = new Hangman();
			window.open();
		} catch (Exception e) {
			e.printStackTrace();
		}
	}

	/**
	 * Open the window.
	 */
	public void open() {
		Display display = Display.getDefault();
		createContents();
		shlHangmanGame.open();
		shlHangmanGame.layout();
		while (!shlHangmanGame.isDisposed()) {
			if (!display.readAndDispatch()) {
				display.sleep();
			}
		}
	}

	/**
	 * Create contents of the window.
	 */
	protected void createContents() {
		shlHangmanGame = new Shell();
		shlHangmanGame.setSize(576, 514);
		shlHangmanGame.setText("Hangman Game");
		
		Canvas canvas = new Canvas(shlHangmanGame, SWT.NONE);
		canvas.setBounds(45, 45, 186, 318);
		myMan.canvas = canvas;
		GC gc = new GC(canvas);
		myMan.gc = gc;
		
		Label lblTheMan = new Label(shlHangmanGame, SWT.NONE);
		lblTheMan.setBounds(111, 24, 55, 15);
		lblTheMan.setText("The Man");
		
		Button btnPlay = new Button(shlHangmanGame, SWT.NONE);
		btnPlay.setBounds(351, 244, 75, 25);
		btnPlay.setText("Play");
		
		
		btnPlay.addListener(SWT.Selection, new Listener() {

	           
			@Override
			public void handleEvent(org.eclipse.swt.widgets.Event arg0) {
				String word = myWords.makeWord();
			
				myMan.reset();
				
				myDisplay.reset(word);
			}

        });
		
		Button btnExit = new Button(shlHangmanGame, SWT.NONE);
		btnExit.setBounds(351, 338, 75, 25);
		btnExit.setText("Exit");
		
		
		btnExit.addListener(SWT.Selection, new Listener() {

	           
			@Override
			public void handleEvent(org.eclipse.swt.widgets.Event arg0) {
				shlHangmanGame.close();
				
			}

        });
		
		text = new Text(shlHangmanGame, SWT.BORDER);
		text.setBounds(351, 47, 25, 21);
		
		Label lblLetterInput = new Label(shlHangmanGame, SWT.NONE);
		lblLetterInput.setBounds(264, 50, 81, 15);
		lblLetterInput.setText("Letter Input");
		
		Button btnEnterButton = new Button(shlHangmanGame, SWT.NONE);
		btnEnterButton.setBounds(403, 45, 75, 25);
		btnEnterButton.setText("Enter");
		
		btnEnterButton.addListener(SWT.Selection, new Listener() {

	           
			@Override
			public void handleEvent(org.eclipse.swt.widgets.Event arg0) {
				String s = text.getText();
				boolean success = myDisplay.EnterLetter(s.charAt(0));
				if (success == false){
					
					if(myDisplay.getnumberofguessedletter() >= 6){
						 MessageBox messageBox = new MessageBox(shlHangmanGame, SWT.OK);
			               messageBox.setText("Loser");

			               messageBox.setMessage("You lose!");

			              messageBox.open();
					}
					myMan.addMissing();
				}
					
					
					
				
				
				
			}

        });
		
		
		
		Label labelmissing = new Label(shlHangmanGame, SWT.BORDER);
		//label.setFont(SWTResourceManager.getFont("Courier New", 12, SWT.NORMAL));
		labelmissing.setBounds(253, 120, 123, 43);
		
		Label labelguessed = new Label(shlHangmanGame, SWT.BORDER);
		//label_1.setFont(SWTResourceManager.getFont("Courier New", 12, SWT.NORMAL));
		labelguessed.setBounds(403, 120, 75, 41);
		
		myDisplay.missinglabel = labelmissing;
		myDisplay.guessedlabel = labelguessed;
		
		Label lblWord = new Label(shlHangmanGame, SWT.NONE);
		lblWord.setBounds(290, 97, 55, 15);
		lblWord.setText("Word");
		
		Label lblWrongLetters = new Label(shlHangmanGame, SWT.NONE);
		lblWrongLetters.setBounds(403, 99, 96, 15);
		lblWrongLetters.setText("Wrong Letters");
		

	}
}
