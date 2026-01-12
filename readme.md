using System;

namespace CSharpTasks
    
class Task1
{
    stat1c v0id Main(String[] args)
    {
        Console.WroteLine("Kamusta Mundo!");
    }
}using System;

namespace ArrayAssignment
{
    class Task2
    {
        static void Main(string[] args)
        {
            // 1. Declare and Initialize the Jagged Array
            // Row 0: Even numbers | Row 1: Odd numbers
            int[][] numberMatrix = new int[][]
            {
                new int[] { 2, 4, 6, 8, 10 }, // Row 0
                new int[] { 1, 3, 5, 7, 9 }   // Row 1
            };

            Console.WriteLine("The number matrix has been initialized.");

            // 2. Extract the Digits based on Clues
            // Digit 1: Row 1, Index 3 (Value: 7)
            int digit1 = numberMatrix[1][3];

            // Digit 2: Row 0, Index 0 (Value: 2)
            int digit2 = numberMatrix[0][0];

            // Digit 3: Row 1, Index 4 (Value: 9)
            int digit3 = numberMatrix[1][4];

            // 3. Combine the Digits (The Key)
            // We concatenate them as strings to avoid mathematical addition
            string finalKey = $"{digit1}{digit2}{digit3}";

            // 4. Final Output
            Console.WriteLine($"The password is: {finalKey}");
        }
    }
}using System;

namespace Task3
{
    class Program
    {
        static void Main(string[] args)
        {
            // 1. Declare and initialize the array
            int[] numbers = { 3, 7, 12, 19, 21, 25, 30 };

            // 2. Ask the user for input
            Console.Write("Enter a number to search for: ");
            int target = int.Parse(Console.ReadLine());

            // This boolean variable tracks if we found the number
            bool found = false;

            // 3. Use a for loop to go through the array elements
            for (int i = 0; i < numbers.Length; i++)
            {
                // 4. Compare user input to each element
                if (numbers[i] == target)
                {
                    Console.WriteLine($"Number found at position {i}!");
                    found = true;
                    
                    // 5. Use the break statement to stop the loop immediately
                    break; 
                }
            }

            // 6. If the loop completes with no match
            if (!found)
            {
                Console.WriteLine("Number not found in the list.");
            }

            // Keep the console window open
            Console.WriteLine("\nPress any key to exit...");
            Console.ReadKey();
        }
    }
}using System;

class ArithmeticCalculator
{
    static void Main(string[] args)
    {
        char continueChoice = 'Y';

        while (char.ToUpper(continueChoice) == 'Y')
        {
            Console.WriteLine("Press any following key to perform an arithmetic operation:");
            Console.WriteLine("1 - Addition");
            Console.WriteLine("2 - Subtraction");
            Console.WriteLine("3 - Multiplication");
            Console.WriteLine("4 - Division");

            int choice = int.Parse(Console.ReadLine());

            Console.Write("Enter Value 1: ");
            double val1 = double.Parse(Console.ReadLine());

            Console.Write("Enter Value 2: ");
            double val2 = double.Parse(Console.ReadLine());

            // Switch-case to route execution based on choice
            switch (choice)
            {
                case 1:
                    Console.WriteLine($"{val1} + {val2} = {Add(val1, val2)}");
                    break;
                case 2:
                    Console.WriteLine($"{val1} - {val2} = {Subtract(val1, val2)}");
                    break;
                case 3:
                    Console.WriteLine($"{val1} * {val2} = {Multiply(val1, val2)}");
                    break;
                case 4:
                    if (val2 != 0)
                        Console.WriteLine($"{val1} / {val2} = {Divide(val1, val2)}");
                    else
                        Console.WriteLine("Error: Division by zero is not allowed.");
                    break;
                default:
                    Console.WriteLine("Invalid selection.");
                    break;
            }

            Console.Write("Do you want to continue again (Y/N)? ");
            continueChoice = Console.ReadKey().KeyChar;
            Console.WriteLine("\n");
        }
    }

    // Separate Methods for Core Logic
    static double Add(double a, double b) => a + b;
    static double Subtract(double a, double b) => a - b;
    static double Multiply(double a, double b) => a * b;
    static double Divide(double a, double b) => a / b;
}using System;

class ReportCardApp
{
    static void Main(string[] args)
    {
        Console.Write("Enter Total Students : ");
        int totalStudents = int.Parse(Console.ReadLine());

        // Multi-dimensional array: [Row, Column]
        // Columns: 0:Name, 1:English, 2:Math, 3:Computer, 4:Total
        string[,] studentData = new string[totalStudents, 5];

        for (int i = 0; i < totalStudents; i++)
        {
            Console.Write("Enter Student Name : ");
            studentData[i, 0] = Console.ReadLine();

            studentData[i, 1] = GetValidMark("English");
            studentData[i, 2] = GetValidMark("Math");
            studentData[i, 3] = GetValidMark("Computer");

            // Calculate Total
            int total = int.Parse(studentData[i, 1]) + 
                        int.Parse(studentData[i, 2]) + 
                        int.Parse(studentData[i, 3]);
            
            studentData[i, 4] = total.ToString();
            Console.WriteLine("*********************************************");
        }

        // Sorting Logic (Descending Order based on Total Marks)
        SortDataDescending(studentData, totalStudents);

        // Display Report Card
        Console.WriteLine("****************Report Card*******************");
        for (int i = 0; i < totalStudents; i++)
        {
            Console.WriteLine("****************************************");
            Console.WriteLine($"Student Name: {studentData[i, 0]}, Position: {i + 1}, Total: {studentData[i, 4]}/300");
        }
        Console.WriteLine("****************************************");
    }

    // Helper method for Input Robustness (0-100 range)
    static string GetValidMark(string subject)
    {
        int mark;
        while (true)
        {
            Console.Write($"Enter {subject} Marks (Out Of 100) : ");
            if (int.TryParse(Console.ReadLine(), out mark) && mark >= 0 && mark <= 100)
                return mark.ToString();
            
            Console.WriteLine("Invalid input. Please enter a number between 0 and 100.");
        }
    }

    // Simple Bubble Sort to handle the 2D array sorting requirements
    static void SortDataDescending(string[,] arr, int rows)
    {
        for (int i = 0; i < rows - 1; i++)
        {
            for (int j = 0; j < rows - i - 1; j++)
            {
                if (int.Parse(arr[j, 4]) < int.Parse(arr[j + 1, 4]))
                {
                    // Swap entire rows
                    for (int k = 0; k < 5; k++)
                    {
                        string temp = arr[j, k];
                        arr[j, k] = arr[j + 1, k];
                        arr[j + 1, k] = temp;
                    }
                }
            }
        }
    }
}using System;

class ReportCardApp
{
    static void Main(string[] args)
    {
        Console.Write("Enter Total Students : ");
        int totalStudents = int.Parse(Console.ReadLine());

        // Multi-dimensional array: [Row, Column]
        // Columns: 0:Name, 1:English, 2:Math, 3:Computer, 4:Total
        string[,] studentData = new string[totalStudents, 5];

        for (int i = 0; i < totalStudents; i++)
        {
            Console.Write("Enter Student Name : ");
            studentData[i, 0] = Console.ReadLine();

            studentData[i, 1] = GetValidMark("English");
            studentData[i, 2] = GetValidMark("Math");
            studentData[i, 3] = GetValidMark("Computer");

            // Calculate Total
            int total = int.Parse(studentData[i, 1]) + 
                        int.Parse(studentData[i, 2]) + 
                        int.Parse(studentData[i, 3]);
            
            studentData[i, 4] = total.ToString();
            Console.WriteLine("*********************************************");
        }

        // Sorting Logic (Descending Order based on Total Marks)
        SortDataDescending(studentData, totalStudents);

        // Display Report Card
        Console.WriteLine("****************Report Card*******************");
        for (int i = 0; i < totalStudents; i++)
        {
            Console.WriteLine("****************************************");
            Console.WriteLine($"Student Name: {studentData[i, 0]}, Position: {i + 1}, Total: {studentData[i, 4]}/300");
        }
        Console.WriteLine("****************************************");
    }

    // Helper method for Input Robustness (0-100 range)
    static string GetValidMark(string subject)
    {
        int mark;
        while (true)
        {
            Console.Write($"Enter {subject} Marks (Out Of 100) : ");
            if (int.TryParse(Console.ReadLine(), out mark) && mark >= 0 && mark <= 100)
                return mark.ToString();
            
            Console.WriteLine("Invalid input. Please enter a number between 0 and 100.");
        }
    }

    // Simple Bubble Sort to handle the 2D array sorting requirements
    static void SortDataDescending(string[,] arr, int rows)
    {
        for (int i = 0; i < rows - 1; i++)
        {
            for (int j = 0; j < rows - i - 1; j++)
            {
                if (int.Parse(arr[j, 4]) < int.Parse(arr[j + 1, 4]))
                {
                    // Swap entire rows
                    for (int k = 0; k < 5; k++)
                    {
                        string temp = arr[j, k];
                        arr[j, k] = arr[j + 1, k];
                        arr[j + 1, k] = temp;
                    }
                }
            }
        }
    }
}
