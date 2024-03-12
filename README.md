    class Program
    {
        private static readonly ILog log = LogManager.GetLogger(typeof(Program));

        static void Main(string[] args)
        {
            if (args.Length < 1)
            {
                Console.WriteLine("Usage: Log4NetConsoleApp <log_file_name>");
                return;
            }

            // Load log4net configuration
            string configFilePath = "log4net.config";
            if (!File.Exists(configFilePath))
            {
                Console.WriteLine($"log4net configuration file '{configFilePath}' not found.");
                return;
            }

            XmlDocument log4netConfig = new XmlDocument();
            log4netConfig.Load(File.OpenRead(configFilePath));

            // Modify log4net configuration to set log file name
            var repository = LogManager.CreateRepository("Log4NetConsoleApp");
            XmlConfigurator.Configure(repository, log4netConfig["log4net"]);

            var hierarchy = (log4net.Repository.Hierarchy.Hierarchy)repository;
            var appenders = hierarchy.Root.Appenders;

            // Assuming there's only one appender defined in the config file
            var fileAppender = (log4net.Appender.FileAppender)appenders[0];
            fileAppender.File = $"{args[0]}.log";
            fileAppender.ActivateOptions();

            // Logging
            log.Info("This is a log message.");

            Console.WriteLine("Logging done. Press any key to exit...");
            Console.ReadKey();
