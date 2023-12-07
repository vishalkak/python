        string currentAssemblyName = Assembly.GetEntryAssembly().GetName().Name;

        // Check if the current program is running
        Process[] existingProcesses = Process.GetProcessesByName(currentAssemblyName);

        if (existingProcesses.Length > 1) // The current process is always in the list
        {
            // Wait for other instances to exit
            foreach (var process in existingProcesses)
            {
                if (process.Id != Process.GetCurrentProcess().Id)
                {
                    Console.WriteLine($"Waiting for another instance of {currentAssemblyName} (Process ID: {process.Id}) to exit...");
                    process.WaitForExit();
                    Console.WriteLine($"Another instance of {currentAssemblyName} has exited.");
                }
            }
        }
        else
        {
            Console.WriteLine($"No other instance of {currentAssemblyName} is currently running.");
        }

        // Continue with the rest of your program logic
        Console.WriteLine("Your program continues here.");
