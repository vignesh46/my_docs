```java


import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;
import org.apache.poi.ss.usermodel.*;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;

public class TextToExcelConverter {
    public static void main(String[] args) {
        String inputFolderPath = "path_to_input_folder";
        String outputFilePath = "path_to_output_excel_file.xlsx";

        List<String> extractedTexts = new ArrayList<>();

        // 1. Read text files from the input folder
        File folder = new File(inputFolderPath);
        File[] files = folder.listFiles();
        if (files != null) {
            for (File file : files) {
                if (file.isFile() && file.getName().endsWith(".txt")) {
                    try {
                        FileInputStream inputStream = new FileInputStream(file);
                        byte[] buffer = new byte[(int) file.length()];
                        inputStream.read(buffer);
                        inputStream.close();
                        String text = new String(buffer);

                        // 2. Extract text between '%' and '/'
                        String extractedText = extractTextBetweenPercentAndSlash(text);
                        extractedTexts.add(extractedText);
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }

        // 3. Write extracted text to an Excel file
        writeTextToExcel(outputFilePath, extractedTexts);
    }

    private static String extractTextBetweenPercentAndSlash(String text) {
        // Implement your logic to extract text between '%' and '/'
        // For example:
        int startIndex = text.indexOf('%') + 1;
        int endIndex = text.indexOf('/');
        if (startIndex >= 0 && endIndex >= 0 && endIndex > startIndex) {
            return text.substring(startIndex, endIndex).trim();
        } else {
            return "";
        }
    }

    private static void writeTextToExcel(String filePath, List<String> textList) {
        Workbook workbook = new XSSFWorkbook();
        Sheet sheet = workbook.createSheet("Extracted Text");

        int rowNum = 0;
        for (String extractedText : textList) {
            Row row = sheet.createRow(rowNum++);
            Cell cell = row.createCell(0);
            cell.setCellValue(extractedText);
        }

        try {
            FileOutputStream outputStream = new FileOutputStream(filePath);
            workbook.write(outputStream);
            outputStream.close();
            System.out.println("Excel file created successfully!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

















import java.io.BufferedWriter;
import java.io.File;
import java.io.FileWriter;
import java.io.IOException;
import java.util.ArrayList;
import java.util.List;

public class TextToFileConverter {
    public static void main(String[] args) {
        String inputFolderPath = "path_to_input_folder";
        String outputFilePath = "path_to_output_text_file.txt";

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

                        // 2. Extract text between '%' and '/'
                        String extractedText = extractTextBetweenPercentAndSlash(text);
                        extractedTexts.add(extractedText);
                    } catch (IOException e) {
                        e.printStackTrace();
                    }
                }
            }
        }

        // 3. Write extracted text to a text file
        writeTextToFile(outputFilePath, extractedTexts);
    }

    private static String extractTextBetweenPercentAndSlash(String text) {
        // Implement your logic to extract text between '%' and '/'
        // For example:
        int startIndex = text.indexOf('%') + 1;
        int endIndex = text.indexOf('/');
        if (startIndex >= 0 && endIndex >= 0 && endIndex > startIndex) {
            return text.substring(startIndex, endIndex).trim();
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
            System.out.println("Text file created successfully!");
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}

```