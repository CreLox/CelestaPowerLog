# Instructions
1. Follow the [CELESTA and CELESTA quattro Light Engine® Instruction Manual](https://cms.lumencor.com/system/uploads/fae/file/asset/48/57-10015-F_Celesta_09092021.pdf) (3.3.3 Ethernet Connection and Control GUI) to set up LAN communication with the onboard computer of the Light Engine. Check if the power regulator is configured to compensate for power reading crosstalk (see the [Light Engine Command Reference](https://cms.lumencor.com/system/uploads/fae/file/asset/120/57-10018.pdf)).

2. Run ``LogLaserPower.ps1`` on Windows PowerShell. You might need to change the URL in the script based on the IP shown on your Light Engine or [change the execution policies of PowerShell](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies) to run the script. The script crawls answer strings from the Light Engine through its REST interface. It requires one parameter input to specify the laser line to be logged. For example, to log the 4th laser line (note that the numbering starts from 0th):

   ```
   LogLaserPower.ps1 4
   ```
   Channel mapping for the Light Engine in the Joglekar Lab: 0-Violet (405 nm), 1-Blue (446 nm), 2-Cyan (477 nm), 3-Teal (520 nm), 4-Green (546 nm), 5-Red (638 nm), and 6-NIR (749 nm).

3. Keep the PowerShell session on for the entire logged session. The log will be on the current user's desktop. It is suggested to add **the exposure time** and **the percentage intensity** information (of the channel that used this laser line) manually to the filename of the log.

4. Each line in the log contains the following formatted information: **hour**(24-h clock)**:min:second**(1-ms accuracy)**,power**(in milliwatts). During most of the time, the laser is off and the instantaneous power readout is simply 0.0 mW. All measurements below ``$Threshold`` mW are ignored and not logged. The idle interval between consecutive queries is ``$Interval`` ms (which does not include the latency due to communication and internal processing). **I am currently working on improving the robustness of queries if no answer string is received within 50 ms.**

5. Run the MATLAB function ``reconstitutePulse`` using the log file to visually examine: (1) the inverse empirical cumulative distribution of instantaneous powers (assuming that all pulses from the session are equivalent) and (2) a scatter plot of instantaneous power readout vs time. **I am currently working on reconstructing the average pulse (which is inevitably smoothed because the power readout can never actually be instantaneous).**

6. Parameters in ``LogLaserPower.ps1`` and ``reconstitutePulse.m`` are closely related. If you change a parameter in a script, you may need to consider changing the related parameter in the other script.
# Acknowledgement
I would like to give my special thanks to Dr. Iain Johnson from Lumencor, Inc. for detailed explanations of terminology involved.
