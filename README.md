# CyberBlog
Used Microsoft Azure to build and host my own "cyber-blog" web application. I secured it with a SSL certificate, and applied Azure's security features to protect it. 

**Walk Through:**

**Part 1: Creating an Azure Web App**
  1. Login into your personal Azure account. If you have not already created an account, go ahead and create one.  https://portal.azure.com.
  2. Select “App Services” from the Azure search field at the top of the page:
  3. Select “+ Create” to create your application and select Web App:
  4. Under the “Basics” tab, make the following selections:
     - Subscription/Resource Group: Select the same subscription and resource group that you used during Cloud         week.
     - Name: Name your instance as you see fit; note that this will be the name of the Azure app.
         For example: “Bobssecurityresume2”
     - Publish: Select “Code.”
     - Runtime Stack: Select “PHP 8.2”
     - Operating System: Select “Linux.”
     - Region: Select the region that you are in
     The following image shows the completed “Basics” tab:

<img width="516" alt="CreateWebAPP" src="https://github.com/CaptainIndy/CyberBlog/assets/142528700/3d3fa79c-48d1-4816-bde8-91af919a49cf">

  5. For the App Service Plan, complete the following steps:
     - Under “Linux Plan,” select “Create New” and then enter “project1plan”.
     - Under “Pricing Plan” select “Explore pricing plans.”
     - From “Select App Service Pricing Plan” select Basic B1 and click “Select”
  6. Leave the default options for all of the other tabs. Select the “Review + Create” tab.
  7. Select “Create” at the bottom of the screen to create your web app.

**Part 2a: Choose a Domain with GoDaddy**
1. Navigate to GoDaddy.com.
2. Pick a website for your cyber-blog, but note the following:
   - The 99 cent domains are usually “.club” or “.xyz,” and “.info” is $1.99 a year.
   - After you pick the domain, the cost may increase after your first year—check carefully for this.
   - We recommend choosing a domain such as “bobscyberblog.club,” but you can pick any domain that you prefer:
3. Once you have chosen an available domain, click “Make it yours,”
4. GoDaddy will offer alternatives that you can consider at different prices.
5. Click “Looks Good, Keep Going” to continue
6. If you do not have a GoDaddy account, you will be prompted to create one.
7. You DO NOT need Domain Protection or a custom email address for this project.
   - Continue to cart
8. Select a duration of one year to get the discounted price, then select “I’m Ready to Pay” and then follow the step to complete your payment.

**Part 2b: Map Your Custom Domain to Azure's App Services**
Here you will point the DNS to Azure's app service.
1. Return to the app that you created within Azure App Service.
2. Select “Custom domains” from the left-hand menu
  - Under Domain provider - Select “All other domain services”
  - Under TLS/SSL certificate - Select Add certificate later
  - Under Domain, enter the Domain you chose from GoDaddy
3. After selecting “Validate,” the page will confirm whether the hostname is available.
   - Select the hostname record type “A Record,” and notice the TXT and A data under “Domain ownership,”
   - You will update this data in the GoDaddy DNS record to prove that you own the domain.
4. Return to your GoDaddy.com products page: https://account.godaddy.com/products.
5. On this page, find the domain that you just added (you may have to scroll down), and select “DNS,”
6. Below your existing DNS records, select “ADD,”
7. One at a time, add in the two “Domain ownership” records (TXT and A), that you saw in Step 3, and click “Save” after each one.
  - The TTL can stay at 1 hour.
8. Once you’ve added both records, they should appear at the bottom of your DNS page
9. Delete the A record that is labeled as "Parked."
10. Return to Azure, and repeat Step 2. After clicking “Validate,” green check marks should appear next to “Hostname availability” and “Domain ownership.”
11. Select “Add” at the bottom of the screen
12. After a minute, your new domain will appear under “Assigned Custom Domains,”
    - If the domain doesn't appear, refresh your page and clear your cache.

**Part 3: Deploy a Container on the Web App**
Using Azure Cloud Shell, we will deploy a Docker container for our web application. 
1. For your web application, you will use a Docker container that has been added to Docker Hub. View the Docker container at the following location: (https://hub.docker.com/r/cyberxsecurity/project1-apachewebserver)
2. Note that the Docker container image name is cyberxsecurity/project1-apachewebserver
3. Next, you will use the Azure Cloud Shell to deploy this container to your web application.
   - Azure Cloud Shell takes user input from a command line to manage Azure's cloud resources.
   - While we will use Bash, you can also use Powershell to administer your commands.
   - To open Azure Cloud Shell, click the shell logo in the tool bar at the top of the screen (first little box with an arrow to the right of the search bar)
   - Once you've clicked this icon, the Cloud Shell will be accessible at the bottom of your page.
   - NOTE - the first time you do this, you will have to create a persistent storage mount (There will be small cost that will initially come out of your credits)
   - When using Shell, you may receive the following prompts:
   - Select which shell to use (Bash or Powershell): Select “Bash.”
   - Create Storage: If a window appears, select “Create Storage.”
4. Next, from the command line, you’ll enter a command to configure your container.
   - There are three types of commands that manage your web app container settings:
        1. _az webapp config container delete_ - This will delete your web app container’s settings.
        2. _az webapp config container set_ - This will set your web app container’s settings.
        3. _az webapp config container show_ - This will display the current details of your web app 
        container’s settings.
   - To configure your web app with your provided container, run the following: _az webapp config container set --name <name of your webapp> --resource-group <name of your resource group> --docker-custom-image-name <container-name> --enable-app-service-storage -t_
- For example: _az webapp config container set --name bobssecurityresume2 --resource-group redteamRG --docker-custom-image-name cyberxsecurity/project1-apachewebserver --enable-app-service-storage -t_
- After pressing enter, an output similar to the image below should appear:
  
<img width="436" alt="Screenshot 2024-01-22 at 2 02 53 PM" src="https://github.com/CaptainIndy/CyberBlog/assets/142528700/20a8e26c-2401-4d36-92ba-c5b929161f12">

  - IMPORTANT - If you get the error “(ResourceNotFound)” verify that the webapp name and resource group are      correct (Case sensitive)

5. To verify that the container has been added correctly, run the following command to show the container for your web app: _az webapp config container show --name <name of webapp> --resource-group <name of your resource group>_
  - For example: _az webapp config container show --name bobssecurityresume2 --resource-group redteamRG_
6. Now, check the unique domain that you selected to verify that the container has been successfully deployed.
  - A cyber blog webpage should appear (note that it may take 5-8 minutes to load)

**Part 4: Designing our Custom Web Application**
The container that we just loaded onto our web application is a framework for a cyber-blog page that we can customize. 
We can now customize the following elements of the webpage:
- Your name
- Your email
- Your LinkedIn profile link
- Your introduction
- Your picture
- Two custom blog posts on topics of your choice

1. To design and customize your webpage, you'll need to access the HTML pages of your new web application.
   - To access these pages, you need to SSH over to your container and access the HTML files.
   - Return to your web app in Azure, select “SSH” from the left-hand toolbar, and then select “GO,”
2. This will SSH you right into the container.
   - Once you have access, change directories to the location where the HTML files are located by running _cd /var/www/html_
3. This directory contains the index.html file that makes up your webpage. To customize your webpage, complete the following steps:
  - To change your name:
      - Run: _nano index.html_
      - Replace “ROBERT SMITH'S CYBER BLOG” with your name/text.
      - Replace “Hi, I'm Robert!” with your name/text.
  
  - To change your email:
    - In the same index.html file, replace “aaggarwal@2u.com” with your email address.
  
  - To change your LinkedIn profile link:
    - In the same index.html file, replace “https://www.linkedin.com/” with the link to your LinkedIn profile.
  
  - To change your introduction:
    - In the same index.html file, replace the paragraph beginning “This is a little introductory paragraph”        with your own introduction.
  
  - To change your picture:.
      - Note that if you prefer not to use a photo of yourself, you can replace it with a stock photo. To do 
      so, replace <img src="https://drive.google.com/uc?export=view&id=1xvxRGAACLqLEMWaw6X_VatbirrIOtepy" 
      with this: <img src="https://image.shutterstock.com/mosaic_250/549673/1198362232/stock-photo-hacking- 
      and-malware-concept-hacker-using-abstract-laptop-with-binary-code-digital-interface-1198362232.jpg"
**Backing up your HTML**
  - Restarting your virtual machine will often clear out any updates to your HTML files. Therefore, it is 
  important to back them up every time you make an update!
  
  - After each update to your webpage, use the following command to backup your index.html file to your /home 
  directory, which stays persistent across reboots.
    - _cp /var/www/html/index.html /home_
   - In case you need to restore your index.html file, run the following command:
    - _cp /home/index.html /var/www/html/_
**Part 5: Creating a Key Vault**
1. In your Personal Azure account, select "Key Vaults" from the Azure search field at the top of the page
2. Select "+ Create" from the Key Vault page to create your key vault
3. On the "Create key vault" tab, make the following selections:
  - Subscription/Resource Group: Select the same subscription and resource groups that you selected prior.
  - Key Vault Name: Choose a key vault name, such as project1-KeyVault. (Note: This name must be globally 
    unique, so you will be prompted to choose a different name if the one you enter has been used before.)
  - Region: Select the same region that you selected on prior.
  - Pricing tier: Select the "Standard" tier.
4. Click next to the Access Configuration Tab
  - Select Vault Access Policy
  - Check the box next to your user name
  - Click Review + Create
5. After your key vault has been created, select your new resource to view your new key vault.
6. Preview the options available on your key vault to store secure information, including:
  - Keys
  - Secrets
  - Certificates

**Part 6: Create a Self-Signed Certificate**
Now we will return to the command line to create a self-signed certificate using OpenSSL. 

1. From our Azure portal, we will return to the command line. Access that same Cloud Shell from earlier that we used to load our Docker container. 
2. From the command line enter the following command in the following format with your own information:
   _openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout <privatekeyname.key> -out <certificatename.crt> -addext "extendedKeyUsage=serverAuth"_

  For example with our information it would be the following: 
    _openssl req -x509 -sha256 -nodes -days 365 -newkey rsa:2048 -keyout project1_key.key -out project1_cert.crt -addext "extendedKeyUsage=serverAuth"_
      -We added the following options:
          -x509: Indicates for OpenSSL to create an SSL certificate.
          -sha256: Uses the sha256 hashing algorithm.
          -nodes
          -days 365: States the certificate will be valid for one year.
          -newkey rsa:2048: Uses a 2048-bit RSA key.
          -keyout project1_key.key: The outputted name of the private key.
          -out project1_cert.crt: The outputted name of the certificate.
          -addext "extendedKeyUsage=serverAuth": Indicates how a public key can be used.
3. After pressing Enter, you will be asked several questions about your certificate. Answer the following:
    - Country Name (2 letter code) [AU]: Enter your country.
    - State or Province Name (full name) [Some State]: Enter your state.
    - Locality Name (e.g., city) [ ]: Enter your city.
    - Organization Name (e.g., company) [Internet Widgits Pty Ltd]: Enter "Student".
    - Organizational Unit Name (e.g., section) [ ]: Leave blank by pressing Enter.
    - Common Name (e.g., server FQDN or YOUR name) [ ]: Enter your full domain name, such as "bobsblog.com".
    - Email Address [ ]: Leave blank by pressing Enter.
4. Now, view your newly created key (.key) and certificate (.crt) by running _ls
  - Note that Azure requires a PFX format for its certificates. The PFX format is the server certificate and 
    the private key combined into a single encrypted file.
5. To create a PFX format, run the following command format:
    - _openssl pkcs12 -export -out <new_certificatename.pfx> -inkey <keyname.key> -in <certificename.crt>_
    - For example: _openssl pkcs12 -export -out project1_cert.pfx -inkey project1_key.key -in 
      project1_cert.crt_
    - We added the following options:
      - pkcs12: Indicates for OpenSSL to create a PFX certificate.
      - export -out project1_cert.pfx: States what to name the PFX file.
      - inkey project1_key.key: This is the current private key that you are importing.
      - in project1_cert.crt: This is the current certificate that you are importing.
6. After pressing Enter, you will be prompted for a password to encrypt your PFX key. Don't forget your password, as you will be prompted for it again shortly! View your new PFX certificate by running _ls
7. To download your new PFX certificate, complete the following four steps:
  (1) Click the "Upload/Download" icon in the toolbar above your Cloud Shell window.
  (2) Select "Download."
  (3) Enter the name of your PFX certificate in the "Download a file" window.
  (4) Click "Download."

**Part 7: Import and Bind your Self-Signed Certificate to Your Web App**
Here we will use Azure to import and bind the certificate we just added to our web application

1. From the Azure Portal, select "Key Vaults."
  - Select the key vault that we created prior .
2. From our key vault, select "Certificates" and then "+ Generate/Import,"
3. On the "Create a certificate" page, select the following:
    - Method of Certificate Creation: Import
    - Certificate Name: project1PFX-cert
    - Upload Certificate File: Select your PFX certificate (it's likely in your Downloads folder)
    - Password: Enter the password that we created earlior
4. Select "Create" to upload your certificate.
  - The following success message should appear to confirm that your PFX certificate has been uploaded to 
    your key vault:
5. Now that you have uploaded your certificate, it's time to add it to your web application. To do so, complete the following steps:
- Return to the web application (under "App Services") that you created prior. 
- On this page, select "Certificates":
6. On this page, import your new PFX certificate from your key vault. To do so, complete the following steps:
   - Select "Bring your own certificates."
   - Click "+ add certificate."
   - Under source, select Import from Key Vault
   - Select the Key Vault you created earlier and the Certificate that you imported
7. Click Select and Validate as shown in the image below

<img width="556" alt="Screenshot 2024-01-22 at 2 58 02 PM" src="https://github.com/CaptainIndy/CyberBlog/assets/142528700/a92bbcc8-1c46-44d0-a534-87a019c61307">

8. Return to Custom Domains and click Add Binding
9 .From the Add TLS/SSL binding screen, select your certificate from the dropdown. Leave TLS/SSL Type at SNI SSL and click “add”
10. Now, open a browser and access your web application. Did you web browser return an error? The error message may look slightly different depending on your browser.
11. Let's examine the certificate that you just added. Click "Not secure" in the search bar if you are in Chrome, or a similar message depending on your browser.
  - After selecting "Not secure," select "Certificate (Invalid)" from the menu to examine your certificate.
  - **Note the reason for your error based on the message on your certificate. This message is due to the fact that your certificate was created by you and not a trusted CA.**
12. Next, click the "Details" tab of your certificate, then select the "Subject" option
    - Note the results that now display in the box on the bottom; these were the options that you selected when you created your certificate with OpenSSL.

**Part 8: Create and Bind an App Service Managed Certificate**
Here we will use Azure's managed certificate to create and bind a more secure certificate to our web apps.
We were just able to create and bind our own self-signed certificate to your web application to encrypt our web traffic. However, unfortunately, our browser displayed warnings to visitors that our website is not trusted and that there may be security risks associated with our web application.

1. First, return to "Certificates" under your web application.
2. Select "Managed Certificates."
3. Select "+ Add Certificate."
4. When the pop-up appears on the right side of your screen, select your domain and click "Create" and then "Validate".
5. Click Add (NOTE - It may take up to 10 minutes to validate and add the managed certificate)
6. Return to Custom Domains, select Add binding next your domain name and select update binding
7. Select the Certificate you just created and click update
8. Now that your new app services managed certificate has been bound to your web application, revisit your website. You should not see any warnings displayed this time! Our Web apps are now secured with a trusted SSL certificate

**Part 9: Create a Front Door Instance**
1. In our Azure portals, access the app service resource we created earlier.
2. From the menu on the left side of the screen, select “Networking.”
3. From this page, select “Azure Front Door” under “Inbound networking features"
4. On the next page, since you haven't created your Front Door resource yet, select “Create new” under “Front Door profile.”
5. This will open a pane on the right side of your screen.
    - In this pane, name your Front Door “project1-FrontDoor”.
    - Enter “project1” for the Endpoint name.
    - The Endpoint hostname will be created automatically based off of the Endpoint name.
    - Enter in “RedTeam” for the Origin group name.
    - Select “Premium” for the Pricing Tier.
    - Click the “OK” button at the bottom of the pane
6. This will return you to the Azure Front Door page.
- Click “Create” to update the Front Door instance to your application.
7. To verify that your Front Door instance has been set up correctly, select “Azure Front Door” (from previous step) again.
8. The message “Azure Front Door is configured for your web app” should display as confirmation

**Part 10: Analyze WAF Rule Sets**
Here we will view the features that are provided by your web application firewall 
1. From our Azure portal, enter "web app" until “Web Application Firewall policies (WAF)” appears as one of the choices in the dropdown.
2. Select that option. The WAF that you created during the previous step should display on the “Web Application Firewall policies (WAF)” page.
  - Note: It will begin with “project1frontdoor” and end with several random letters and numbers.
3. Select your WAF and as the WAF policies page opens, notice the options on the left side of yoru screen
4. Select “Managed rules” either from the left-hand toolbar or from the box on the bottom of the page. When this page appears, scroll through the page to view the various rules.
    - Note the following about these rules:
      - This is the list of the application vulnerabilities that the WAF will protect against
      - While it’s unlikely that your web application would be impacted by these vulnerabilities, it is   
        important to note these Azure WAF features, which identify and blocks the application attacks 
        indicated on this page.
      - These managed rules can be individually enabled or disabled, and a variety of actions can be taken if 
         an attack is identified, such as:
          - Allow the request.
          - Block the request.
          - Log the request.
          - Redirect the request to another webpage.
            
**Part 11: Configure Custom WAF Rules**
Here we will configure a custom WAF rule to protet against a potential security attack. Let’s assume for this project that you have been experiencing a variety of attacks from international IP addresses, and you need to only accept traffic from the locations where your business partners reside: the United States, Canada, and Australia.
1. Select “Custom rules” from the toolbar on the left-hand side of the screen
2. To create a custom rule, select “+ Add custom rule.”
  - When the pane pops up on the right, name your custom rule “Project1rule”.
  - Leave the status and rule type at the default options.
  - Set the priority to 100.
  - Set the following terms for the rule’s condition:
    - Match type: Geo location
    - Match variable: Remoteaddr
    - Operation: is not
    - Select the three countries (USA, Canada, Australia)
    - Then: Deny traffic

<img width="561" alt="Screenshot 2024-01-22 at 3 30 07 PM" src="https://github.com/CaptainIndy/CyberBlog/assets/142528700/8bd3e221-8dba-4a4b-9265-5e65e9fe8a6d">

3. Then, click “Add,” and your custom rule should now display on the page,


**Part 12: Analyze and Fix a Security Center Recommendation**
Here we will go over how to use Azure Security Center to analyze and fix a recommendation from the Security Center dashboard. 

1. To access Azure Security Center, from your web app, select “Microsoft Defender for Cloud” from the toolbar on the left.
2. When the Security Center page opens, it should display counts for both recommendations and alerts (note that your counts may vary).
  - Review the recommendations, and note that Azure describes the recommendations in this way: “Security Center continuously monitors the configuration of your app services to identify potential security vulnerabilities and recommends actions to mitigate them.”
3. If your recommendations do not display, below is an example of security recommendations which may appear in this section.

<img width="651" alt="Screenshot 2024-01-22 at 3 36 15 PM" src="https://github.com/CaptainIndy/CyberBlog/assets/142528700/f970b17b-f3d7-49b6-abce-68322a9aac2d">

4. Note that when you select on an individual recommendation, it will then display the recommended steps to remediate this recommendation.

**Part 13: Conclusion**
Here is a screenshot of my cyberblog I made before I took it down because I did not want to pay for Azure's services. I will redeploy it locally soon and I recommend that after completing this project as a way to post projects, blog about topics you're interested in. GitHub is also great for hosting your own website but this a fun alternative. 



<img width="653" alt="Screenshot 2024-01-22 at 3 40 19 PM" src="https://github.com/CaptainIndy/CyberBlog/assets/142528700/234612d7-e8d9-4f52-b3de-f5b2bf86fa85">
















