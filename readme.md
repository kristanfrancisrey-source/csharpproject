using System;

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
}
