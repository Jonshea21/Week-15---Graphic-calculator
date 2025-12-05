using System;

namespace EnhancedCalculator
{
    // A separate class to handle the calculator logic (OOP Principle: Encapsulation)
    public class Calculator
    {
        // Method for basic binary operations (+, -, *, /)
        public double PerformOperation(double num1, double num2, string operation)
        {
            switch (operation)
            {
                case "+":
                    return num1 + num2;
                case "-":
                    return num1 - num2;
                case "*":
                    return num1 * num2;
                case "/":
                    if (num2 == 0)
                    {
                        // Custom exception for division by zero (Innovative/Code Quality)
                        throw new DivideByZeroException("Cannot divide by zero.");
                    }
                    return num1 / num2;
                default:
                    throw new ArgumentException("Invalid binary operation.");
            }
        }

        // Methods for single-number functions (Innovative/Functionality)
        public double SquareRoot(double num)
        {
            if (num < 0)
            {
                throw new ArgumentException("Cannot calculate the square root of a negative number.");
            }
            return Math.Sqrt(num);
        }

        // Method for power (Innovative/Functionality)
        public double Power(double baseNum, double exponent)
        {
            return Math.Pow(baseNum, exponent);
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            // Instantiate the Calculator class (OOP Principle: Object Creation)
            Calculator calculator = new Calculator(); 
            bool continueCalculating = true;

            Console.WriteLine("=== Enhanced Console Calculator (Supports +, -, *, /, sqrt, pow) ===\n");

            while (continueCalculating)
            {
                try
                {
                    Console.Write("Enter first number or function (e.g., 10, sqrt, pow): ");
                    string input1 = Console.ReadLine().ToLower().Trim();

                    // Check for single-argument functions
                    if (input1 == "sqrt" || input1 == "sqr")
                    {
                        Console.Write("Enter number for square root: ");
                        double num = Convert.ToDouble(Console.ReadLine());
                        double result = calculator.SquareRoot(num);
                        Console.WriteLine($"\nResult: sqrt({num}) = {result}");
                    }
                    else // Assume it's the first number for a binary operation or power
                    {
                        double num1 = Convert.ToDouble(input1); // Use the input as the first number

                        Console.Write("Enter operation (+, -, *, /, pow): ");
                        string operation = Console.ReadLine().ToLower().Trim();

                        Console.Write("Enter second number (or exponent for 'pow'): ");
                        double num2 = Convert.ToDouble(Console.ReadLine());

                        double result = 0;
                        if (operation == "pow") // Handle power as a special binary case
                        {
                            result = calculator.Power(num1, num2);
                            Console.WriteLine($"\nResult: {num1}^{num2} = {result}");
                        }
                        else // Handle basic binary operations
                        {
                            result = calculator.PerformOperation(num1, num2, operation);
                            Console.WriteLine($"\nResult: {num1} {operation} {num2} = {result}");
                        }
                    }
                }
                catch (FormatException)
                {
                    Console.WriteLine("\nError: Please enter a valid number or function keyword.");
                }
                catch (DivideByZeroException dbze) // Catch custom exception
                {
                    Console.WriteLine($"\nError: {dbze.Message}");
                }
                catch (ArgumentException ae) // Catch validation errors from the Calculator class
                {
                    Console.WriteLine($"\nError: {ae.Message}");
                }
                catch (Exception ex)
                {
                    Console.WriteLine($"\nAn unexpected error occurred: {ex.Message}");
                }

                // Ask if user wants to continue
                Console.Write("\nDo you want to perform another calculation? (y/n): ");
                string response = Console.ReadLine().ToLower();
                continueCalculating = (response == "y" || response == "yes");
                Console.WriteLine();
            }

            Console.WriteLine("Thank you for using the calculator! Goodbye.");
        }
    }
}
