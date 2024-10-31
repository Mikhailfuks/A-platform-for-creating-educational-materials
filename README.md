using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text;

namespace EducationalMaterialPlatform
{
    public class EducationalMaterialPlatform
    {
        private readonly string _materialsDirectory;

        public EducationalMaterialPlatform(string materialsDirectory)
        {
            _materialsDirectory = materialsDirectory;
            if (!Directory.Exists(_materialsDirectory))
            {
                Directory.CreateDirectory(_materialsDirectory);
            }
        }

        public void CreateCourse(string courseName)
        {
            var courseDirectory = Path.Combine(_materialsDirectory, courseName);
            if (!Directory.Exists(courseDirectory))
            {
                Directory.CreateDirectory(courseDirectory);
                Console.WriteLine($"Course '{courseName}' created successfully.");
            }
            else
            {
                Console.WriteLine($"Course '{courseName}' already exists.");
            }
        }

        public void CreateModule(string courseName, string moduleName)
        {
            var moduleDirectory = Path.Combine(_materialsDirectory, courseName, moduleName);
            if (!Directory.Exists(moduleDirectory))
            {
                Directory.CreateDirectory(moduleDirectory);
                Console.WriteLine($"Module '{moduleName}' created successfully in course '{courseName}'.");
            }
            else
            {
                Console.WriteLine($"Module '{moduleName}' already exists in course '{courseName}'.");
            }
        }

        public void AddLesson(string courseName, string moduleName, string lessonName, string content)
        {
            var lessonFile = Path.Combine(_materialsDirectory, courseName, moduleName, lessonName + ".txt");
            File.WriteAllText(lessonFile, content);
            Console.WriteLine($"Lesson '{lessonName}' added successfully to module '{moduleName}' in course '{courseName}'.");
        }

        public void ListCourses()
        {
            var courses = Directory.EnumerateDirectories(_materialsDirectory);
            if (courses.Any())
            {
                Console.WriteLine("Available Courses:");
                foreach (var course in courses)
                {
                    Console.WriteLine($"- {Path.GetFileName(course)}");
                }
            }
            else
            {
                Console.WriteLine("No courses found.");
            }
        }

        public void ListModules(string courseName)
        {
            var courseDirectory = Path.Combine(_materialsDirectory, courseName);
            if (Directory.Exists(courseDirectory))
            {
                var modules = Directory.EnumerateDirectories(courseDirectory);
                if (modules.Any())
                {
                    Console.WriteLine($"Modules in course '{courseName}':");
                    foreach (var module in modules)
                    {
                        Console.WriteLine($"- {Path.GetFileName(module)}");
                    }
                }
                else
                {
                    Console.WriteLine($"No modules found in course '{courseName}'.");
                }
            }
            else
            {
                Console.WriteLine($"Course '{courseName}' not found.");
            }
        }

        public void ViewLesson(string courseName, string moduleName, string lessonName)
        {
            var lessonFile = Path.Combine(_materialsDirectory, courseName, moduleName, lessonName + ".txt");
            if (File.Exists(lessonFile))
            {
                var content = File.ReadAllText(lessonFile);
                Console.WriteLine($"Lesson '{lessonName}' content:");
                Console.WriteLine(content);
            }
            else
            {
                Console.WriteLine($"Lesson '{lessonName}' not found.");
            }
        }
    }

    public class Program
    {
        static void Main(string[] args)
        {
            var platform = new EducationalMaterialPlatform("EducationalMaterials");

            // Create a course
            platform.CreateCourse("Programming 101");

            // Create modules
            platform.CreateModule("Programming 101", "Fundamentals");
            platform.CreateModule("Programming 101", "Data Structures");

            // Add lessons
            platform.AddLesson("Programming 101", "Fundamentals", "Introduction", "Welcome to Programming 101! This lesson covers the basics of programming...");
            platform.AddLesson("Programming 101", "Data Structures", "Arrays", "Arrays are used to store collections of data...");

            // List courses
            platform.ListCourses();

            // List modules
            platform.ListModules("Programming 101");

            // View a lesson
            platform.ViewLesson("Programming 101", "Fundamentals", "Introduction");

            Console.ReadKey();
        }
    }
}
