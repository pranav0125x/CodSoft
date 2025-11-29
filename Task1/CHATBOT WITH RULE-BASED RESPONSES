import java.awt.*;
import java.net.URI;
import java.net.URLEncoder;
import java.util.*;
import javax.swing.*;
   
class ChatBotAISimple extends JFrame {
    private JTextArea chatArea;
    private JTextField inputField;
    private JButton sendButton;
    private String pendingAction = null;
    private String pendingInput = null;

    public ChatBotAISimple() {
        setTitle("ChatBot AI");
        setSize(600, 500);
        setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        setLayout(new BorderLayout());

        chatArea = new JTextArea();
        chatArea.setEditable(false);
        chatArea.setLineWrap(true);
        chatArea.setWrapStyleWord(true);
        chatArea.setFont(new Font("Arial", Font.PLAIN, 14));
        chatArea.setMargin(new Insets(10, 10, 10, 10));
        JScrollPane scrollPane = new JScrollPane(chatArea);
        add(scrollPane, BorderLayout.CENTER);

        JPanel inputPanel = new JPanel(new BorderLayout(5, 5));
        inputPanel.setBorder(BorderFactory.createEmptyBorder(10, 10, 10, 10));
        
        inputField = new JTextField();
        inputField.setFont(new Font("Arial", Font.PLAIN, 14));
        inputPanel.add(inputField, BorderLayout.CENTER);

        sendButton = new JButton("Send");
        sendButton.setFont(new Font("Arial", Font.BOLD, 14));
        inputPanel.add(sendButton, BorderLayout.EAST);

        add(inputPanel, BorderLayout.SOUTH);

        chatArea.append("ChatBot: Hey there! I'm your chatbot buddy. I can chat, joke, help you out, tell you the weather, check the date, time, and day, and even do simple maths.\n");
        chatArea.append("ChatBot: What can I do for you today?\n");
        chatArea.append("ChatBot: (type 'exit' or 'end' to end the chat).\n\n");

        sendButton.addActionListener(e -> processInput());
        inputField.addActionListener(e -> processInput());

        setLocationRelativeTo(null);
        setVisible(true);
        inputField.requestFocus();
    }

    private void processInput() {
        String input = inputField.getText().trim();
        if (input.isEmpty()) {
            return;
        }

        chatArea.append("You: " + input + "\n");
        inputField.setText("");

        if (pendingAction != null) {
            handlePendingAction(input);
            return;
        }

        if (input.equals("exit") || input.equals("end")) {
            chatArea.append("ChatBot: Bye! I'll be here, recharging my virtual brain.\n");
            chatArea.append("ChatBot: :)\n");
            sendButton.setEnabled(false);
            inputField.setEnabled(false);
            return;
        }

        String result = reply(input);
        chatArea.append("ChatBot: " + result + "\n\n");
        chatArea.setCaretPosition(chatArea.getDocument().getLength());
    }

    private void handlePendingAction(String input) {
        String i = input.toLowerCase();
        
        if (pendingAction.equals("weather_confirm")) {
            if (i.contains("yes")) {
                pendingAction = "weather_city";
                chatArea.append("ChatBot: Type your city:\n");
            } else {
                chatArea.append("ChatBot: Aborted\n\n");
                pendingAction = null;
            }
        } else if (pendingAction.equals("weather_city")) {
            try {
                String url = "https://www.accuweather.com/en/search-locations?query=" + 
                    URLEncoder.encode(input, "UTF-8");
                Desktop.getDesktop().browse(new URI(url));
                chatArea.append("ChatBot: Opening weather report in your browser...\n\n");
            } catch (Exception e) {
                chatArea.append("ChatBot: Couldn't open browser.\n\n");
            }
            pendingAction = null;
        } 
        else if (pendingAction.equals("time_confirm")) {
            if (i.contains("yes")) {
                pendingAction = "time_country";
                chatArea.append("ChatBot: Type your country/city:\n");
            } else {
                chatArea.append("ChatBot: Aborted\n\n");
                pendingAction = null;
            }
        } 
        else if (pendingAction.equals("time_country")) {
            try {
                String url = "https://time.is/" + URLEncoder.encode(input, "UTF-8");
                Desktop.getDesktop().browse(new URI(url));
                chatArea.append("ChatBot: Opening time information in your browser...\n\n");
            } catch (Exception e) {
                chatArea.append("ChatBot: Couldn't open browser.\n\n");
            }
            pendingAction = null;
        }
        
        chatArea.setCaretPosition(chatArea.getDocument().getLength());
    }

    private static String extractName(String input) {
        input = input.toLowerCase();
        input = input.replace("my name is", "").trim();

        if(input.isEmpty()){
            return "Human";
        }
        return input.substring(0,1).toUpperCase() + input.substring(1);
    }

    private static String calcMat(String in){
        String res = in.replaceAll("[^0-9+\\-*/]", " ").trim();
        try{
            if(res.contains("+")) {
                String[] parts = res.split("\\+");
                return "The result is: " + (Integer.parseInt(parts[0]) + Integer.parseInt(parts[1]));
            } 
            if(res.contains("-")) {
                String[] parts = res.split("-");
                return "The result is: " + (Integer.parseInt(parts[0]) - Integer.parseInt(parts[1]));
            } 
            if(res.contains("*")) {
                String[] parts = res.split("\\*");
                return "The result is: " + (Integer.parseInt(parts[0]) * Integer.parseInt(parts[1]));
            } 
            if(in.contains("/")) {
                String[] parts = in.split("/");
                int a = Integer.parseInt(parts[0]);
                int b = Integer.parseInt(parts[1]);
                
                if (b == 0){
                    return"Cannot divide by zero!";
                }
                return "The result is: " + ((double)a / b);
            }
        }
        
        catch(Exception e){
            System.out.println("I do simple maths, not rocket science. :(");
        }
        return "Please use '+', '-', '*', or '/'";
    }

    private String reply(String in){
        String i = in.toLowerCase();
        Random r = new Random();
        
        if(i.contains("hi") || i.contains("hey") || i.contains("hello") || i.contains("greetings") || i.contains("good morning") || i.contains("good afternoon") || i.contains("good evening")){
            String[] str = {
                "Hello there! How can i assist you today?",
                "Hi there! What can I do for you?",
                "Greetings! How may I assist you?"
            };
            return str[r.nextInt(str.length)];
        }
        if (i.matches(".*my name is.*")) {
            String name = extractName(i);
            return "Nice to meet you, " + name + "!";
        }
        if(i.contains("how are you")){
            return "I'm all good. My life is just ones and zeros.";
        }
        if(i.contains("who are you")){
            return "I'm your friendly neighbourhood chatbot. No spider powers though.";
        }
        if(i.contains("weather") || i.contains("forecast") || i.contains("temperature")){
            pendingAction = "weather_confirm";
            return "If you would like to know today's weather/forecast/temperature, the website will open automatically in your browser.\nIf you want to continue, type yes/no.";
        }
        if(i.contains("time") || i.contains("date") ||i.contains("day")){
            pendingAction = "time_confirm";
            return "If you would like to know the current date/day/time, the website will open automatically in your browser.\nIf you want to continue, type yes/no.";
        }
        if(i.contains("thanks") || i.contains("thank you") || i.contains("thank") || i.contains("appreciate")){
            String[] thanks = {
                "You're very welcome! Let me know if you need anything else.",
                "Happy to help. Is there anything else I can do for you today?",
                "Glad I could assist! Feel free to ask if you have more questions."
            };
            return thanks[r.nextInt(thanks.length)];
        }
        if(i.contains("joke") || i.contains("jokes") || i.contains("funny") || i.contains("more")) {
            String[] joke = {
                "Why don't programmers like nature? It has too many bugs.",
                "Why did the computer catch a cold? It left its Windows open.",
                "Why was the Java developer broke? Because he used up all his cache.",
                "I told my laptop a joke. It didn't laugh. It just crashed.",
                "Why do coders prefer dark mode? Because light attracts bugs.",
                "Why did the scarecrow become a successful developer? He was outstanding in his field.",
                "Why don't robots panic? They have nerves of steel.",
                "What's a computer's favorite snack? Microchips.",
                "Why do phones ring? Because they can't talk.",
                "Why was the math book sad? It had too many problems."
            };
            return joke[r.nextInt(joke.length)];
        }
        if(i.contains("+")|| i.contains("-") || i.contains("*") || i.contains("/")){
            try {
                return calcMat(i);
            } 
            catch (Exception e) {
                return "I couldn't calculate that. Try simple expressions";
            }
        }

        String[] basic= {
            "I'm sorry, I didn't quite catch that. Could you say it again?",
            "I'm not sure I understand. Can you rephrase that?",
            "Oops, I didn't get that. What was that?",
            "Sorry, I'm drawing a blank on that one. Can you say it another way?"
        };
        return basic[r.nextInt(basic.length)];
    }

    public static void main(String args[]){
        SwingUtilities.invokeLater(() -> new ChatBotAISimple());
    }
}
