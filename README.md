import java.util.*;
import java.io.*;

// ======================================================
// =============== CO4: CLASS + ABSTRACT CLASS + CONSTRUCTOR + ENCAPSULATION
// ======================================================
abstract class Question {
    protected String questionText;
    protected int correctAnswer;

    // CO4 Topic 1: Constructor
    public Question(String questionText, int correctAnswer){
        this.questionText = questionText;
        this.correctAnswer = correctAnswer;
    }

    // CO4 Topic 2: Abstract methods (polymorphic behavior)
    public abstract void display();

    public abstract boolean checkAnswer(int userAns);

    // CO4 Topic 3: Encapsulation using getter
    public String getQuestionText() {
        return this.questionText;
    }
}

// ======================================================
// =============== CO5: INHERITANCE + POLYMORPHISM + METHOD OVERRIDING
// ======================================================
class MCQQuestion extends Question {
    private String[] options;

    // CO5 Topic 1: super keyword
    public MCQQuestion(String text, String[] options, int ans){
        super(text, ans);
        this.options = options;
    }

    // CO5 Topic 2: Method overriding
    @Override
    public void display(){
        System.out.println(questionText);
        for (int i = 0; i < options.length; i++){
            System.out.println((i+1) + ". " + options[i]);
        }
    }


    @Override
    public boolean checkAnswer(int userAns){
        return userAns == correctAnswer;
    }
}

// ======================================================
// =============== QUIZ SYSTEM
// ======================================================
class Quiz {

    // CO2 Topic 1: ArrayList (Collections)
    private ArrayList<Question> questionList = new ArrayList<>();

    private int score = 0;

    // ======================================================
    // =============== Load Questions
    // ======================================================
    // CO2 Topic 2: Arrays
    public void loadQuestions(){

        String[] q1Options = {"Storing files", "Encrypting data", "Directing internet traffic", "Managing passwords"};
        String[] q2Options = {"CPU", "Hard Drive", "RAM", "GPU"};
        String[] q3Options = {"2000", "2004", "2006", "2008"};

        // CO3 Topic 1: String operations
        String message = "loading questions...";
        System.out.println(message.toUpperCase()); // Convert to uppercase

        // CO3 Topic 2: Bitwise operations
        int correctAns = 3 | 0;

        //  CO5 Topic 3: Polymorphism: List stores object of subclass
        questionList.add(new MCQQuestion("What is the function of a router?", q1Options, correctAns));
        questionList.add(new MCQQuestion("Which part is the brain of the computer?", q2Options, 1 | 0));
        questionList.add(new MCQQuestion("What year was Facebook launched?", q3Options, 2 | 0));

        // CO2 Topic 3: Iteration through arrays used during quizzes
    }

    // ======================================================
    // =============== Start Quiz
    // ======================================================
    public void startQuiz(){

        Scanner sc = new Scanner(System.in);

        for (Question q : questionList){
            q.display();
            int ans = getValidInput(sc);

            if(q.checkAnswer(ans)){
                score++;
                System.out.println("Correct!\n");
            } else {
                System.out.println("Wrong!\n");
            }
        }

        System.out.println("Final Score = " + score + "/" + questionList.size());

        // CO6 Topic 1: Exception handling
        try {
            saveScore(score);
        } catch (Exception e){
            System.out.println("Error saving score.");
        }
    }

    // ======================================================
    // =============== CO3: RECURSION + INPUT VALIDATION + STRING MATCHING
    // ======================================================
    private int getValidInput(Scanner sc){
        try {
            System.out.print("Enter option number: ");
            return sc.nextInt();
        }

        catch (Exception e){
            sc.nextLine();
            System.out.println("Invalid input! Numbers only.");

            // CO3 Topic 3: Recursion
            return getValidInput(sc);
        }
    }

    // ======================================================
    // =============== CO6: FILE I/O + EXCEPTION HANDLING + STREAMS
    // ======================================================
    private void saveScore(int s) throws IOException {

        // CO6 Topic 2: FileWriter + BufferedWriter
        BufferedWriter bw = new BufferedWriter(new FileWriter("score.txt", true));

        // CO6 Topic 3: Using Java Stream to convert score to String
        String scoreLine = Arrays.asList("Score: ", String.valueOf(s))
                .stream()
                .reduce("", (a,b) -> a + b);

        bw.write(scoreLine + "\n");
        bw.close();
    }
}

// ======================================================
// =============== MAIN (CO1 topics used here)
// ======================================================
public class Main {
    public static void main(String[] args){

        // CO1 Topic 1: Variables & datatypes
        int start = 1;

        // CO1 Topic 2: Output formatting + System I/O
        System.out.println("=== Online Quiz System ===");

        // CO1 Topic 3: Conditional statements
        if(start == 1){
            Quiz quiz = new Quiz();
            quiz.loadQuestions();
            quiz.startQuiz();
        }
    }
}
