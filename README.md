import acm.graphics.*;
import acm.util.RandomGenerator;

public class HangmanCanvas extends GCanvas {

	/** Resets the display so that only the scaffold appears */
	public void reset() {
		// drawing scaffold, which will be shown at the start of the game
		GLine scaffold = new GLine(20, 20, 20, 20 + SCAFFOLD_HEIGHT);
		add(scaffold);
		GLine beam = new GLine(20, 20, 20 + BEAM_LENGTH, 20);
		add(beam);
		GLine rope = new GLine(20 + BEAM_LENGTH, 20, 20 + BEAM_LENGTH, 20 + ROPE_LENGTH);
		add(rope);
	}

	/**
	 * Updates the word on the screen to correspond to the current state of the
	 * game. The argument string shows what letters have been guessed so far;
	 * unguessed letters are indicated by hyphens.
	 */
	public void displayWord(String word) {
		double x = 20 + BEAM_LENGTH - HIP_WIDTH / 2 - FOOT_LENGTH;
		double y = 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH + 60;
		GLabel guessedWord = new GLabel(word, x, y);
		guessedWord.setFont("-20");
		// if there already is a word, it removes it and adds a new one
		if (getElementAt(x, y) != null) {
			remove(getElementAt(x, y));
		}
		add(guessedWord);
	}

	/**
	 * Updates the display to correspond to an incorrect guess by the user. Calling
	 * this method causes the next body part to appear on the scaffold and adds the
	 * letter to the list of incorrect guesses that appears at the bottom of the
	 * window.
	 */
	// creating different parts of body
	private void head() {
		GOval head = new GOval(20 + BEAM_LENGTH - HEAD_RADIUS / 2, 20 + ROPE_LENGTH, HEAD_RADIUS, HEAD_RADIUS);
		add(head);
	}

	private void body() {
		GLine body = new GLine(20 + BEAM_LENGTH, 20 + ROPE_LENGTH + HEAD_RADIUS, 20 + BEAM_LENGTH,
				20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH);
		add(body);
		GLine hip = new GLine(20 + BEAM_LENGTH - HIP_WIDTH / 2, 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH,
				20 + BEAM_LENGTH + HIP_WIDTH / 2, 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH);
		add(hip);
	}

	private void left_arm() {
		GLine arm_offset = new GLine(20 + BEAM_LENGTH - UPPER_ARM_LENGTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + ARM_OFFSET_FROM_HEAD, 20 + BEAM_LENGTH + UPPER_ARM_LENGTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + ARM_OFFSET_FROM_HEAD);
		add(arm_offset);
		GLine left_arm = new GLine(20 + BEAM_LENGTH - UPPER_ARM_LENGTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + ARM_OFFSET_FROM_HEAD, 20 + BEAM_LENGTH - UPPER_ARM_LENGTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + ARM_OFFSET_FROM_HEAD + LOWER_ARM_LENGTH);
		add(left_arm);
	}

	private void right_arm() {
		GLine right_arm = new GLine(20 + BEAM_LENGTH + UPPER_ARM_LENGTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + ARM_OFFSET_FROM_HEAD, 20 + BEAM_LENGTH + UPPER_ARM_LENGTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + ARM_OFFSET_FROM_HEAD + LOWER_ARM_LENGTH);
		add(right_arm);
	}

	private void left_leg() {
		GLine left_leg = new GLine(20 + BEAM_LENGTH - HIP_WIDTH / 2, 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH,
				20 + BEAM_LENGTH - HIP_WIDTH / 2, 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH);
		add(left_leg);
	}

	private void right_leg() {
		GLine right_leg = new GLine(20 + BEAM_LENGTH + HIP_WIDTH / 2, 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH,
				20 + BEAM_LENGTH + HIP_WIDTH / 2, 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH);
		add(right_leg);
	}

	private void left_foot() {
		GLine left_foot = new GLine(20 + BEAM_LENGTH - HIP_WIDTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH,
				20 + BEAM_LENGTH - HIP_WIDTH / 2 - FOOT_LENGTH,
				20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH);
		add(left_foot);
	}

	private void right_foot() {
		GLine right_foot = new GLine(20 + BEAM_LENGTH + HIP_WIDTH / 2,
				20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH,
				20 + BEAM_LENGTH + HIP_WIDTH / 2 + FOOT_LENGTH,
				20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH);
		add(right_foot);
	}

	public void noteIncorrectGuess(String wrongLetters) {
		//adds wrong letters under the guessed word
		double x = 20 + BEAM_LENGTH - HIP_WIDTH / 2 - FOOT_LENGTH;
		double y = 20 + ROPE_LENGTH + HEAD_RADIUS + BODY_LENGTH + LEG_LENGTH + 90;
		GLabel incorrectLetters = new GLabel(wrongLetters, x, y);
		if (getElementAt(x, y) != null) {
			remove(getElementAt(x, y));
		}
		incorrectLetters.setFont("-15");
		// slowly adds parts of the body if user doesn't guess characters
		add(incorrectLetters);
		if (wrongLetters.length() == 1) {
			head();
		}
		if (wrongLetters.length() == 2) {
			body();
		}
		if (wrongLetters.length() == 3) {
			left_arm();
		}
		if (wrongLetters.length() == 4) {
			right_arm();
		}
		if (wrongLetters.length() == 5) {
			left_leg();
		}
		if (wrongLetters.length() == 6) {
			right_leg();
		}
		if (wrongLetters.length() == 7) {
			left_foot();
		}
		if (wrongLetters.length() == 8) {
			right_foot();
		}
	}

	/* Constants for the simple version of the picture (in pixels) */
	private static final int SCAFFOLD_HEIGHT = 360;
	private static final int BEAM_LENGTH = 144;
	private static final int ROPE_LENGTH = 18;
	private static final int HEAD_RADIUS = 36;
	private static final int BODY_LENGTH = 144;
	private static final int ARM_OFFSET_FROM_HEAD = 28;
	private static final int UPPER_ARM_LENGTH = 72;
	private static final int LOWER_ARM_LENGTH = 44;
	private static final int HIP_WIDTH = 36;
	private static final int LEG_LENGTH = 108;
	private static final int FOOT_LENGTH = 28;

}
