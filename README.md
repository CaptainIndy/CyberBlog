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
















