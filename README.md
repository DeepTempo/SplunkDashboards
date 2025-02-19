# Splunk Tempo Dashboard Installer

This project automates the installation of Splunk Enterprise, imports a custom dashboard that displays output from the Snowflake app Tempo project, and sets up an admin user account. The script is designed to work on Amazon EC2 instances running Amazon Linux, as well as other Linux distributions.

## Prerequisites

- An Amazon EC2 instance running Amazon Linux or another compatible Linux distribution
- Root or sudo access
- Splunk Enterprise installer tarball (`.tgz` file) obtained from your authorized Splunk software provider
- `anomaly_hub.xml` dashboard file from this repo

## What the Script Does

1. Locates the Splunk Enterprise installer in the current directory
2. Installs Splunk Enterprise
3. Sets up an admin user and a default user with restricted permissions
4. Configures the firewall to allow access to Splunk's web interface
5. Restarts Splunk to apply all changes

## Usage

1. Clone this repository to your EC2 instance:
   ```
   git clone https://github.com/DeepTempo/SplunkDashboards.git
   cd SplunkDashboards
   ```

2. Place the Splunk Enterprise tarball (e.g., `splunk-*-Linux-x86_64.tgz`) in the same directory as the script.

3. Ensure the `anomaly_hub.xml` file is in the same directory as the script.

4. Open the script in a text editor and replace `'your_admin_password'`, `'default_user'`, and `'default_password'` with your desired credentials:
   ```
   nano splunk_tempo_install.sh
   ```

5. Make the script executable:
   ```
   chmod +x splunk_tempo_install.sh
   ```

6. Run the script with sudo privileges:
   ```
   sudo ./splunk_tempo_install.sh
   ```

7. Go to the splunk at `your_ip:8000` and you should be prompted to login
   <img width="625" alt="Screenshot 2024-09-13 at 2 41 08 PM" src="https://github.com/user-attachments/assets/f389a9bb-40a4-4c78-85c8-548277d22c43">
   
   Use the credentials User:admin Passowrd:password

   Note: Change these defalts!!! Leaving these as is presents a HUGE security risk.
   
8. Next we need to load your data.  To do this take the .csv file you retreaved from the ouput of the Tempo SnowFlake app and download it to your local computer

9. Once you have the file. Return to the Splunk tab in your browser and click settings

<img width="625" alt="Screenshot 2024-09-13 at 2 48 10 PM" src="https://github.com/user-attachments/assets/79aae761-d426-4958-943e-7ec6cc2dd2ce">

10. This will bring up a menu with a giant add data button click the button

<img width="625" alt="Screenshot 2024-09-13 at 2 48 43 PM" src="https://github.com/user-attachments/assets/7fe40f7d-06a1-4f9f-8986-b8723ffcd12e">

11. Select upload and select the .csv file you downloaded from SnowFlake. Use all the default options when loading the CSV by clicking next till you arrive at the Done window.

<img width="625" alt="Screenshot 2024-09-30 at 1 51 18 PM" src="https://github.com/user-attachments/assets/e255ddcb-8d4c-4256-8934-105f4ac79b39">

18. Once the CSV has loaded Click the "Build Dashboards" button
    
    <img width="625" alt="Screenshot 2024-09-30 at 1 52 40 PM" src="https://github.com/user-attachments/assets/5a051b76-9a3a-4bad-ac98-7687f6daa766">

11. Click create new dashboard

<img width="625" alt="Screenshot 2024-09-13 at 2 44 49 PM" src="https://github.com/user-attachments/assets/35db5012-33c3-4c2e-9c8b-baeaa2309bf8">

11. Fill in the fileds and select clasic dashboard builder and click create

<img width="625" alt="Screenshot 2024-09-13 at 2 45 26 PM" src="https://github.com/user-attachments/assets/95f06692-d12e-4336-9ec1-3c676bd1bdcc">
   
12. Click Source

<img width="625" alt="Screenshot 2024-09-13 at 2 46 07 PM" src="https://github.com/user-attachments/assets/72186ecf-93d3-479b-91b8-45c76faa57c3">
   
13. Copy the XML from the anomaly_hub.xml in this repo and paste it in the window.

<img width="625" alt="Screenshot 2024-09-13 at 2 47 36 PM" src="https://github.com/user-attachments/assets/d7a74dd5-4e6a-47f5-8b94-3797bde842dc">

14. Find the line where the csv is loaded. It should look similar to the following
                         `<query>source="2024-09-24 4_03pm.csv" host="Josiah" sourcetype="csv"
| stats count as event_count</query>`

Relace the csv file name with your own 

15. Click Save
    
    <img width="625" alt="Screenshot 2024-09-30 at 1 58 32 PM" src="https://github.com/user-attachments/assets/13f330b6-fa7c-49f3-a59c-41e099db6ca7">

## Important Notes

- The script sets up an admin user
- Ensure you set strong passwords for the admin user in the script before running it.
- Firewall configuration may vary depending on your Linux distribution. The script uses `firewall-cmd`, which is common in Red Hat-based systems. You may need to adjust this for other distributions.
- Ensure you have sufficient disk space for Splunk Enterprise and its data (minimum 10GB recommended).

## Post-Installation

After running the script and setup steps, you can access the Splunk web interface at `http://your_ec2_instance_public_ip:8000`. Log in with either the admin credentials or the default user credentials you set up.

## Troubleshooting

- If the script fails to find the Splunk tarball, ensure it's in the same directory and follows the naming convention `splunk-*.tgz`.
- If you encounter issues with user creation, check the Splunk server logs and ensure you're using the correct authentication credentials in the script.

## Security Considerations

- Always use strong, unique passwords for the admin user account.
- Consider using HTTPS for the Splunk web interface in production environments.
- Review and adjust firewall rules as needed for your security requirements.
- Regularly review user access and permissions to ensure they align with the principle of least privilege.
- Keep your EC2 instance and Splunk Enterprise updated with the latest security patches.

## Support

For issues related to Splunk Enterprise, please refer to [Splunk's official documentation](https://docs.splunk.com/Documentation/Splunk).

For questions about the Snowflake app Tempo project integration, please contact your Snowflake support team or refer to the Tempo project documentation.

If you encounter any issues with this installer script, please open an issue in this GitHub repository.

## Contributing

We welcome contributions to improve this installer script. Please fork the repository, make your changes, and submit a pull request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
