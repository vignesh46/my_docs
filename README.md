```java
import java.io.*;
import java.util.ArrayList;
import java.util.List;
import java.util.regex.Matcher;
import java.util.regex.Pattern;

public class TextFromEachLineExtractor {
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
                        List<String> lines = readLinesFromFile(file);

                        // 2. Extract text from each line based on startWord and endWord
                        for (String line : lines) {
                            String extractedText = extractTextFromLine(line, startWord, endWord);
                            if (!extractedText.isEmpty()) {
                                extractedTexts.add(extractedText);
                            }
                        }
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }

        // 3. Write all extracted text to a single output file
        writeTextToFile(outputFilePath, extractedTexts);
    }

    private static List<String> readLinesFromFile(File file) throws IOException {
        List<String> lines = new ArrayList<>();
        try (BufferedReader reader = new BufferedReader(new FileReader(file))) {
            String line;
            while ((line = reader.readLine()) != null) {
                lines.add(line);
            }
        }
        return lines;
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

    private static void writeTextToFile(String filePath, List<String> textList) {
        String concatenatedText = String.join("\n", textList);
        try (BufferedWriter writer = new BufferedWriter(new FileWriter(filePath))) {
            writer.write(concatenatedText);
            System.out.println("Text file created successfully: " + filePath);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```