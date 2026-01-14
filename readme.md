using System;

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
}
