# CollegeEventWatcher
This program is supposed to view when a program has start Examples being Notepad, Microsoft word, google chrome, etc. it will show what time the program has started and what time the program was closed out. and then this info would be stored in a database

```
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using System.Windows.Forms;
using System.Diagnostics;
using System.Management;
using System.Runtime.InteropServices;
using Microsoft.Win32;

namespace WindowsFormsApp8
{
    public partial class Form1 : Form
    {
        struct APPINFO
        {

        }

        private readonly List<APPINFO> currentlyRunningProcesses = new List<APPINFO>();

        public Form1()
        {
            InitializeComponent();
            ManagementEventWatcher AppStartQuery = new ManagementEventWatcher(new WqlEventQuery("SELECT * FROM Win32_ProcessStartTrace"));
            ManagementEventWatcher AppStopQuery = new ManagementEventWatcher(new WqlEventQuery("SELECT * FROM Win32_ProcessStopTrace"));

            AppStartQuery.EventArrived += new EventArrivedEventHandler(StartedProcess);
            AppStartQuery.Start();

            AppStopQuery.EventArrived += new EventArrivedEventHandler(StoppedProcess);
            AppStopQuery.Start();


            Console.WriteLine("Exit");
            Console.ReadLine();


        }
        private void StartedProcess(object sender, EventArrivedEventArgs e)
        {
            try
            {
                var processStart = Process.GetProcessById(Convert.ToInt32(e.NewEvent.Properties["processID"].Value.ToString()));
                int processID = Process.GetProcessById(processStart.Id).Id;
                string processName = Process.GetProcessById(processStart.Id).ProcessName;
                string name = "Process Name: ";
                //Process[] procname = Process.GetProcessesByName(Convert.ToInt32(e.NewEvent.Properties["notepad"].Value.ToString());
                if (processName == "notepad")
                {
                    ProcessStart.Invoke(new Action(() => {
                        ProcessStart.Text = ProcessStart.Text + DateTime.Now.ToString("MM/dd/yyyy hh:mm tt | ")
                        + name.ToString() + processName.ToString() + processID.ToString(@" ID:0") + Environment.NewLine;
                    }));
                }



                //richtextbox

                /*ProcessStart.Invoke(new Action(() => {
                    ProcessStart.Text = ProcessStart.Text + DateTime.Now.ToString("MM/dd/yyyy hh:mm tt | ")
                    + name.ToString() + processName.ToString() + processID.ToString(@" ID:0") + Environment.NewLine;
                }));*/

            }
            catch (Exception ee)
            {
                Console.WriteLine("Start Process Exception: " + ee.Message);
            }
        }



        private void StoppedProcess(object sender, EventArrivedEventArgs e)
        {
            try
            {

                var processStop = Process.GetProcessById(Convert.ToInt32(e.NewEvent.Properties["processID"].Value.ToString()));

                //int processID = Process.GetProcessById(processStop.Id).Id;

                int processID = Convert.ToInt32(e.NewEvent.Properties["ProcessID"].Value.ToString());
                //string processNamee = Process.GetProcessById(processStop.Id).ProcessName;
                string processName = e.NewEvent.Properties["ProcessName"].Value.ToString();
                string namee = "Process Name: ";
                if (processName == "notepad")
                {
                    ProcessStop.Invoke(new Action(() => {
                        ProcessStop.Text = ProcessStop.Text
                        + DateTime.Now.ToString("MM/dd/yyyy hh:mm tt | ") + namee.ToString() + processName.ToString()
                        + processID.ToString(@"   ID:0") + Environment.NewLine;
                    }));
                }


            }
            catch (Exception ee)
            {
                Console.WriteLine("Stop Process Exception: " + ee.Message);
            }
        }
        private void Form1_Load(object sender, EventArgs e)
        {

        }
    }
}
```
