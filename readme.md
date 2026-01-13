using System;

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
}
