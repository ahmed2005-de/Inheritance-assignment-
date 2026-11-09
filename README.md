# Inheritance-assignment-

using System.ComponentModel;

namespace InheritanceMinigame.Classes
{

    public class Character
    {
        public string Name  {get; set;}
        public int Health  {get; set;}
        public int MaxHealth  {get; set;}
        public int AttackPower{get; set;}

        public bool IsAlive => Health > 0;

                   protected Character(string name, int health, int attackPower)
        {
            Name = name;
            MaxHealth = health;
            Health = health;
            AttackPower = attackPower;
        }
                public virtual void TakeDamage(int damage)
        {
            Health -= damage;
            
            if (Health < 0)
                Health = 0;
            
            Console.WriteLine($"{Name} takes {damage} damage! (HP: {Health}/{MaxHealth})");
        }

        public virtual void PerformAttack (Character target) {}
    } 
}

using System;

namespace InheritanceMinigame.Classes
{
    public class Dragon : Character
    {
                public Dragon(string name) : base(name, 200, 20) { }
                        public override void PerformAttack(Character target)
        {
            Console.WriteLine($"{Name} breathes devastating fire across the battlefield!");
            target.TakeDamage(AttackPower);
        }
    }
}

using System;

namespace InheritanceMinigame.Classes
{
    public class Goblin : Character
    {
        public Goblin(string name) : base(name, 60, 10) { }
                public override void PerformAttack(Character target)
        {
            Console.WriteLine($"{Name} slashes with a rusty dagger!");
            target.TakeDamage(AttackPower);
            
            if (Random.Shared.Next(0, 100) < 30)
            {
                Console.WriteLine($"{Name} strikes a second time out of nowhere!");
                target.TakeDamage(AttackPower / 2);
            }
        }
    }
}

using System;

namespace InheritanceMinigame.Classes
{
    public class Mage : Character
    {
        public int Mana { get; private set; } = 50;
                public Mage(string name) : base(name, 80, 25) { }
    

               public override void PerformAttack(Character target)
        {
            if (Mana >= 20)
            {
                Mana -= 20;
                Console.WriteLine($"{Name} casts a massive Fireball at {target.Name}! (Mana left: {Mana})");
                target.TakeDamage(AttackPower);
            }
            else
            {
                Console.WriteLine($"{Name} is out of mana! They whack {target.Name} weakly with a staff.");
                target.TakeDamage(5); 
            }
        }
    }
}

using System;
using System.Collections.Generic;
using InheritanceMinigame.Classes;

namespace InheritanceMinigame
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("WELCOME TO THE INHERITANCE ARENA");
            Console.Write("Enter your hero's name: ");
            string heroName = Console.ReadLine();
            if (string.IsNullOrWhiteSpace(heroName))
                heroName = "Unknown Hero";

            Console.WriteLine("\nChoose your class:");
            Console.WriteLine("1. Warrior (High Health, Blocks Damage)");
            Console.WriteLine("2. Mage (Fragile, Deals Massive Fireball Damage)");
            Console.Write("Selection (1 or 2): ");
            string choice = Console.ReadLine();

                        Character player;

            if (choice == "2")
                player = new Mage(heroName);
            else
                player = new Warrior(heroName);

                        Queue<Character> dungeonEnemies = new Queue<Character>();
            dungeonEnemies.Enqueue(new Goblin("Gorg the Sneaky"));
            dungeonEnemies.Enqueue(new Goblin("Zib the Vile"));
            dungeonEnemies.Enqueue(new Dragon("Ignis the Flame-Bringer"));

            Console.WriteLine($"{player.Name} enters the dungeon!");

                        while (player.IsAlive && dungeonEnemies.Count > 0)
            {
                Character currentEnemy = dungeonEnemies.Peek();
                Console.WriteLine($"\n--------------------------------------------");
                Console.WriteLine($"💥 A wild {currentEnemy.GetType().Name} appears: {currentEnemy.Name}!");
                Console.WriteLine($"--------------------------------------------");

                while (player.IsAlive && currentEnemy.IsAlive)
                {
                    Console.WriteLine("\nPress ENTER to attack!");
                    Console.ReadLine();
                    player.PerformAttack(currentEnemy);

                    if (!currentEnemy.IsAlive)
                    {
                        Console.WriteLine($"You defeated {currentEnemy.Name}!");
                        dungeonEnemies.Dequeue();
                        break;
                    }

                    currentEnemy.PerformAttack(player);
                }
            }

                        Console.WriteLine("\n--- END OF GAME ---");
            if (player.IsAlive)
                Console.WriteLine("CONGRATULATIONS! You cleared the dungeon and won the game!");
            else
                Console.WriteLine("YOU DID NOT SURVIVE! GAME OVER.");
            Console.WriteLine("========================================");
        }
    }
}

using System;

namespace InheritanceMinigame.Classes
{
    public class Warrior : Character
    {
        public int ShieldBlock { get; set; } = 4;

                public Warrior(string name) : base(name, 120, 15)
        {
            
        }

                public override void PerformAttack(Character target)
        {
            Console.WriteLine($"{Name} swings a heavy greatsword at {target.Name}!");
            target.TakeDamage(AttackPower);
        }
        public override void TakeDamage(int damage)
        {
            int finalDamage = Math.Max(1, damage - ShieldBlock);
            Console.WriteLine($"{Name} blocks {ShieldBlock} damage with their shield!");
            base.TakeDamage(finalDamage);
        }
    }
}
    
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <OutputType>Exe</OutputType>
    <TargetFramework>net10.0</TargetFramework>
    <ImplicitUsings>enable</ImplicitUsings>
    <Nullable>enable</Nullable>
  </PropertyGroup>

</Project>

