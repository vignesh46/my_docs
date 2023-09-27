```java
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class TextBetweenWordsExtractor {
    public static void main(String[] args) {
        String inputFolderPath = "path_to_input_folder";
        String outputFilePath = "path_to_output_file.txt";
        String startWord = "START";
        String endWord = "END";

        List<String> extractedTexts = new ArrayList<>();

        // 1. Read text files from the input folder
        File folder = new File(inputFolderPath);
        File[] files = folder.listFiles();
        if (files != null) {
            for (File file : files) {
                if (file.isFile() && file.getName().endsWith(".txt")) {
                    try {
                        // Read the text from the file
                        String text = readFile(file);

                        // 2. Extract text between startWord and endWord
                        String extractedText = extractTextBetweenWords(text, startWord, endWord);
                        extractedTexts.add(extractedText);
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }

        // 3. Write all extracted text to a single output file
        writeTextToFile(outputFilePath, extractedTexts);
    }

    private static String extractTextBetweenWords(String text, String startWord, String endWord) {
        String pattern = Pattern.quote(startWord) + "(.*?)" + Pattern.quote(endWord);
        Pattern regex = Pattern.compile(pattern, Pattern.DOTALL);
        Matcher matcher = regex.matcher(text);
        if (matcher.find()) {
            return matcher.group(1).trim();
        } else {
            return "";
        }
    }

    private static String readFile(File file) throws IOException {
        StringBuilder content = new StringBuilder();
        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
            String line;
            while ((line = reader.readLine()) != null) {
                content.append(line).append("\n");
            }
        }
        return content.toString();
    }

    private static void writeTextToFile(String filePath, List<String> textList) {
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            for (String extractedText : textList) {
                writer.write(extractedText);
                writer.newLine();
            }
            System.out.println("Text file created successfully: " + filePath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}


```
