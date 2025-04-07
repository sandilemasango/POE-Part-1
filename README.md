using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using NAudio.Wave;
using System;
using System.Collections.Generic;

namespace POE_Part_1
{
    internal class Program
    {
        class CyberAwareBot
        {
            static readonly string asciiLogo = @"
   ____            _                 ____                                 _             
  / ___|___  _ __ | |__   ___ _ __  | __ )  ___  ___ _ ____   _____ _ __ | |_ ___  _ __ 
 | |   / _ \| '_ \| '_ \ / _ \ '__| |  _ \ / _ \/ __| '__\ \ / / _ \ '_ \| __/ _ \| '__|
 | |__| (_) | | | | | | |  __/ |    | |_) |  __/\__ \ |   \ V /  __/ | | | || (_) | |   
  \____\___/|_| |_|_| |_|\___|_|    |____/ \___||___/_|    \_/ \___|_| |_|\__\___/|_|   

                         🤖 Cybersecurity Awareness Bot 🤖
                      Your friendly guide to staying safe online!
";

            static readonly List<string> cyberTips = new List<string>
    {
        "Use strong, unique passwords for each account.",
        "Enable two-factor authentication (2FA) wherever possible.",
        "Be cautious of phishing emails. Don't click suspicious links.",
        "Keep your software and operating system up to date.",
        "Avoid using public Wi-Fi for sensitive activities.",
        "Backup your important data regularly.",
        "Use antivirus software and a firewall."
    };

            static readonly Dictionary<string, string> faqResponses = new Dictionary<string, string>(StringComparer.OrdinalIgnoreCase)
    {
        {"what is phishing", "Phishing is a type of cyber attack where attackers trick you into revealing personal information by pretending to be a trustworthy source."},
        {"what is 2fa", "2FA stands for two-factor authentication, an extra layer of security to verify your identity."},
        {"how to create a strong password", "Use a mix of uppercase, lowercase, numbers, and symbols. Avoid using your name or common words."},
        {"how to stay safe online", "Use secure websites, update your software regularly, avoid suspicious links, and use strong passwords."}
    };

            static void Main()
            {
                Console.OutputEncoding = System.Text.Encoding.UTF8;
                ShowHeader();
                RunChatbot();
            }

            static void ShowHeader()
            {
                Console.Clear();
                Console.ForegroundColor = ConsoleColor.Cyan;
                Console.WriteLine(asciiLogo);
                Console.ResetColor();
                Console.WriteLine("Type 'menu' to begin or 'exit' to quit.\n");
                Console.WriteLine("before we begin, whats your name");
            }

            static void ShowMenu()
            {
                Console.WriteLine("\n🛡️ MENU:");
                Console.WriteLine("1. Ask a question");
                Console.WriteLine("2. Get a cybersecurity tip");
                Console.WriteLine("3. Show all tips");
                Console.WriteLine("4. Exit");
            }

            static void AnswerQuestion()
            {
                Console.Write("\nAsk your cybersecurity question: ");
                string question = Console.ReadLine().Trim().ToLower();

                bool found = false;
                foreach (var entry in faqResponses)
                {
                    if (question.Contains(entry.Key))
                    {
                        Console.WriteLine($"\n🤖 {entry.Value}");
                        found = true;
                        break;
                    }
                }

                if (!found)
                {
                    Console.WriteLine("\n🤖 Sorry, I don't have an answer for that yet.");
                }
            }

            static void GiveRandomTip()
            {
                Random rand = new Random();
                string tip = cyberTips[rand.Next(cyberTips.Count)];
                Console.WriteLine($"\n💡 Tip: {tip}");
            }

            static void ShowAllTips()
            {
                Console.WriteLine("\n📋 Cybersecurity Tips:");
                for (int i = 0; i < cyberTips.Count; i++)
                {
                    Console.WriteLine($"{i + 1}. {cyberTips[i]}");
                }
            }

            static void RunChatbot()
            {
                while (true)
                {
                    Console.Write("\n> ");
                    string input = Console.ReadLine().Trim().ToLower();

                    switch (input)
                    {
                        case "menu":
                            ShowMenu();
                            break;
                        case "1":
                            AnswerQuestion();
                            break;
                        case "2":
                            GiveRandomTip();
                            break;
                        case "3":
                            ShowAllTips();
                            break;
                        case "4":
                        case "exit":
                            Console.WriteLine("\n🔒 Stay safe! Goodbye.");
                            return;
                        default:
                            Console.WriteLine("❓ Unknown command. Type 'menu' to see your options.");
                            break;
                    }
                }
            }
        }
        static void Main(string[] args)
        {
            Console.WriteLine("Please enter a filename");
            string filename = Console.ReadLine();
            filename += ".wav";
            Console.WriteLine("Press any key to start recording...");
            Console.ReadKey();

            var waveFormat = new WaveFormat(44100, 1);

            using (var waveFile = new WaveFileWriter(filename, waveFormat))
            {
                using (var waveIn = new WaveInEvent())
                {
                    waveIn.WaveFormat = waveFormat;

                    waveIn.DataAvailable += (s, e) =>
                    {
                        waveFile.Write(e.Buffer, 0, e.BytesRecorded);
                    };

                    waveIn.StartRecording();
                    Console.WriteLine("Recording... press any key to stop.");
                    Console.ReadKey();

                    waveIn.StopRecording();
                    Console.WriteLine("Recording stopped.");

                }
            }

            Console.WriteLine("Audio recorded and saved as " + filename);
        }
    }
}
