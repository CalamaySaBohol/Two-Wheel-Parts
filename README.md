using System;
using System.Collections.Generic;

namespace TwoWheeledPartsSystem
{
    // Base class for a general part
    public class Part
    {
        public string PartName { get; set; }
        public string PartNumber { get; set; }
        public decimal Price { get; set; }

        public Part(string name, string number, decimal price)
        {
            PartName = name;
            PartNumber = number;
            Price = price;
        }

        // Basic info for inventory list (no price)
        public virtual void DisplayBasicInfo()
        {
            Console.WriteLine($"{PartName} (#{PartNumber})");
        }

        // Full info including price
        public virtual void DisplayInfo()
        {
            Console.WriteLine($"Part: {PartName}, Number: {PartNumber}, Price: ₱{Price}");
        }
    }

    public class EnginePart : Part
    {
        public int HorsePower { get; set; }

        public EnginePart(string name, string number, decimal price, int hp)
            : base(name, number, price)
        {
            HorsePower = hp;
        }

        public override void DisplayBasicInfo()
        {
            Console.WriteLine($"[Engine] {PartName} (#{PartNumber}) - {HorsePower} HP");
        }

        public override void DisplayInfo()
        {
            Console.WriteLine($"[Engine] {PartName} (#{PartNumber}) - {HorsePower} HP, Price: ₱{Price}");
        }
    }

    public class WheelPart : Part
    {
        public int SizeInInches { get; set; }

        public WheelPart(string name, string number, decimal price, int size)
            : base(name, number, price)
        {
            SizeInInches = size;
        }

        public override void DisplayBasicInfo()
        {
            Console.WriteLine($"[Wheel] {PartName} (#{PartNumber}) - {SizeInInches}\"");
        }

        public override void DisplayInfo()
        {
            Console.WriteLine($"[Wheel] {PartName} (#{PartNumber}) - {SizeInInches}\" Size, Price: ₱{Price}");
        }
    }

    class Program
    {
        static void Main(string[] args)
        {
            List<Part> partsInventory = new List<Part>
            {
                new EnginePart("Motorcycle Engine", "ENG001" , 15000m, 100),
                new WheelPart("Front Wheel", "WHL101", 3500m, 17),
                new WheelPart("Rear Wheel", "WHL102", 4000m, 17),
                new EnginePart("Scooter Engine", "ENG002", 12000m, 80),
                new WheelPart("Scooter Front Wheel", "WHL201", 3000m, 14),
                new WheelPart("Scooter Rear Wheel", "WHL202", 3200m, 14),
            };

            List<Part> cart = new List<Part>(); // user’s chosen parts

            while (true)
            {
                Console.Clear();
                Console.WriteLine("=== Two-Wheeled Parts Inventory ===");
                for (int i = 0; i < partsInventory.Count; i++)
                {
                    Console.Write($"{i + 1}. ");
                    partsInventory[i].DisplayBasicInfo(); // show basic info only
                }

                Console.WriteLine("\nOptions:");
                Console.WriteLine("1 - View part details (with price)");
                Console.WriteLine("2 - Add part to cart");
                Console.WriteLine("3 - View cart & total price");
                Console.WriteLine("0 - Exit");
                Console.Write("Enter choice: ");

                string choice = Console.ReadLine();

                if (choice == "0")
                    break;
                else if (choice == "1")
                {
                    Console.Write("Enter part number (1 to {0}): ", partsInventory.Count);
                    if (int.TryParse(Console.ReadLine(), out int partIndex) &&
                        partIndex >= 1 && partIndex <= partsInventory.Count)
                    {
                        Console.Clear();
                        partsInventory[partIndex - 1].DisplayInfo(); // full info with price
                    }
                    else
                    {
                        Console.WriteLine("Invalid choice!");
                    }
                    Console.WriteLine("\nPress any key to continue...");
                    Console.ReadKey();
                }
                else if (choice == "2")
                {
                    Console.Write("Enter part number to add to cart (1 to {0}): ", partsInventory.Count);
                    if (int.TryParse(Console.ReadLine(), out int partIndex) &&
                        partIndex >= 1 && partIndex <= partsInventory.Count)
                    {
                        cart.Add(partsInventory[partIndex - 1]);
                        Console.WriteLine($"{partsInventory[partIndex - 1].PartName} added to cart!");
                    }
                    else
                    {
                        Console.WriteLine("Invalid choice!");
                    }
                    Console.WriteLine("\nPress any key to continue...");
                    Console.ReadKey();
                }
                else if (choice == "3")
                {
                    Console.Clear();
                    Console.WriteLine("=== Your Cart ===");
                    decimal total = 0;
                    foreach (var item in cart)
                    {
                        item.DisplayInfo(); // show price in cart
                        total += item.Price;
                    }
                    Console.WriteLine($"\nTotal Price: ₱{total}");
                    Console.WriteLine("\nPress any key to continue...");
                    Console.ReadKey();
                }
                else
                {
                    Console.WriteLine("Invalid option!");
                    Console.ReadKey();
                }
            }
        }
    }
}
