```java
import java.io.*;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class TextFromEachLineExtractor {
    public static void main(String[] args) {
        String inputFolderPath = "path_to_input_folder";
        String outputFilePath = "path_to_output_file.txt";
        String startWord = "START";
        String endWord = "END";

        // Open the output file for writing
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(outputFilePath))) {

            // 1. Read text files from the input folder
            File folder = new File(inputFolderPath);
            File[] files = folder.listFiles();
            if (files != null) {
                for (File file : files) {
                    if (file.isFile() && file.getName().endsWith(".txt")) {
                        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
                            String line;
                            while ((line = reader.readLine()) != null) {
                                // 2. Extract text from each line based on startWord and endWord
                                String extractedText = extractTextFromLine(line, startWord, endWord);
                                if (!extractedText.isEmpty()) {
                                    // 3. Write extracted text to the output file
                                    writer.write(extractedText);
                                    writer.newLine();
                                }
                            }
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
            }
            
            System.out.println("Text file created successfully: " + outputFilePath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String extractTextFromLine(String line, String startWord, String endWord) {
        String pattern = Pattern.quote(startWord) + "(.*?)" + Pattern.quote(endWord);
        Pattern regex = Pattern.compile(pattern);
        Matcher matcher = regex.matcher(line);
        if (matcher.find()) {
            return matcher.group(1).trim();
        } else {
            return "";
        }
    }
}














import java.io.*;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class TextToCSVConverter {
    public static void main(String[] args) {
        String inputFolderPath = "path_to_input_folder";
        String outputFilePath = "output.csv"; // Output as CSV file
        String startWord = "START";
        String endWord = "END";

        // Open the output CSV file for writing
        try (PrintWriter writer = new PrintWriter(new BufferedWriter(new FileWriter(outputFilePath)))) {

            // Write header row (optional)
            writer.println("Extracted Text");

            // 1. Read text files from the input folder
            File folder = new File(inputFolderPath);
            File[] files = folder.listFiles();
            if (files != null) {
                for (File file : files) {
                    if (file.isFile() && file.getName().endsWith(".txt")) {
                        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
                            String line;
                            while ((line = reader.readLine()) != null) {
                                // 2. Extract text from each line based on startWord and endWord
                                String extractedText = extractTextFromLine(line, startWord, endWord);
                                if (!extractedText.isEmpty()) {
                                    // 3. Write extracted text to the CSV file
                                    writer.println(extractedText);
                                }
                            }
                        } catch (IOException e) {
                            e.printStackTrace();
                        }
                    }
                }
            }

            System.out.println("CSV file created successfully: " + outputFilePath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static String extractTextFromLine(String line, String startWord, String endWord) {
        String pattern = Pattern.quote(startWord) + "(.*?)" + Pattern.quote(endWord);
        Pattern regex = Pattern.compile(pattern);
        Matcher matcher = regex.matcher(line);
        if (matcher.find()) {
            return matcher.group(1).trim();
        } else {
            return "";
        }
    }
}

```