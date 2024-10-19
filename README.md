# Day-27-Investigating-RDP-Brute-Force-Alerts

In this challenge, you’ll learn how to effectively investigate an RDP Brute Force alert and identify the critical indicators to look for during the investigation process. Whether you’re handling security incidents regularly or just starting out, this guide will help you navigate the steps involved in examining suspicious RDP activity.

Let’s jump in!

Just like with SSH Brute Force attacks, the investigative approach for RDP Brute Force attacks follows a similar pattern. Though the protocols differ, the core analysis techniques remain consistent. Additionally, once we’ve configured our alert system to work with OS Ticket, all our alerts will automatically be routed there — streamlining the process and ensuring no critical alert slips through the cracks. This automation will help you manage and track alerts efficiently.

## Step 1: Getting Started with Elastic
Start by opening your Elastic Web GUI and navigating to the Security tab by clicking on the hamburger icon. Scroll down and select Alerts. Depending on the data in your system, the number of alerts may vary.

**Tip:** Always ensure your time frame is set to 30 days for more comprehensive results.

In my case, I have a total of 3,000 alerts, and one IP that stands out is **194.26.135.52**, using the username of **Administrator**.

![Alt text](https://raw.githubusercontent.com/Virus192/Day-27-Investigating-RDP-Brute-Force-Alerts/refs/heads/main/Images/photo_6035328477617570245_w.jpg)

## Step 2: Automating Alerts to OS Ticket
If you remember from our previous blog on SSH Brute Force alerts, we modified the alerts to automatically create a ticket in OS Ticket. We can do the same for RDP Brute Force attacks to streamline alert management.

### Here’s how:
1. **Navigate to Rules:** Go to the Rules section in Elastic and locate the RDP Brute Force rule.
2. **Edit the Rule:** Click on Edit Rules, and at the top, you’ll see an Actions tab.
3. **Select Webhook:** Under actions, select Webhook.
4. **Alert Configuration:** Set up the body code for the alert arrangement and save the changes.
5. **Set the Schedule:** Ensure the rule runs every 1 minute by going to Edit Rule Settings, clicking on Schedule, and setting the frequency.

By doing this, you ensure that every alert is captured in OS Ticket, creating an automatic workflow for investigation.

## Step 3: Investigating the RDP Brute Force Alert
Now, let’s focus on the core part of the investigation. Just like in SSH Brute Force attacks, we need to answer four critical questions:

1. Is the IP known for RDP Brute Force activity?
2. Are there any other users affected by this IP?
3. Were any of the login attempts successful?
4. If successful, what happened after the login?

### 1. Checking the IP — 194.26.135.52
To begin, we’ll check if this IP is known for malicious activity. As expected, the IP **23.164.57.20** is flagged as malicious and is known for performing RDP Brute Force attacks.

![Alt text](https://raw.githubusercontent.com/Virus192/Day-27-Investigating-RDP-Brute-Force-Alerts/refs/heads/main/Images/photo_6035328477617570235_w.jpg)

![Alt text](https://raw.githubusercontent.com/Virus192/Day-27-Investigating-RDP-Brute-Force-Alerts/refs/heads/main/Images/photo_6035328477617570236_w.jpg)


### 2. Identifying Affected Users
Next, let’s investigate if other users are impacted by this IP. In our case, we found that only the **administrator** account has been targeted by this IP.

### 3. Checking for Successful Logins
To determine if any of the login attempts were successful, search for the IP **23.164.57.20** in Elastic’s Discovery section. Use the event code **4624**, which corresponds to a successful login.

In our example, there were no successful logins for this IP. So, we can conclude that no unauthorized access was gained.

However, in a different scenario we had previously tested (using a Mythic Command and Control attack), we did see a successful RDP brute force attempt.

![Alt text](https://raw.githubusercontent.com/Virus192/Day-27-Investigating-RDP-Brute-Force-Alerts/refs/heads/main/Images/photo_6035328477617570244_w.jpg)

### 4. What Happened After the Login?
Let’s explore that earlier test where we successfully logged in:

- **Login Time:** The attacker logged in successfully on September 21, 2024, at 18:23:50.112.
- **End Time:** The session lasted until 21:02:10.629 the same day.

Within this time frame, several things could have occurred:
- **Process Creations:** Did the attacker launch new processes?
- **Persistence Mechanisms:** Did they try to establish a persistent backdoor?
- **Lateral Movement:** Did they attempt to move across the network?
- **Exfiltration Attempts:** Were there any signs of data theft?
- **Command and Control:** Did they communicate with an external server?

These are just a few possibilities to investigate when analyzing the time frame of a successful brute force login. Always keep an eye on unusual behavior, such as command execution, file downloads, or privilege escalation.

## Step 4: Investigating Previous Attacks
As we’ve discussed before, we had previously used Mythic C2 for a simulated RDP brute force attack on a Windows Server. During that investigation, we confirmed:
- **Administrator** was the targeted user.
- The login was successful, and further investigation uncovered potentially suspicious activity within the session.

This highlights the importance of knowing what to look for after a successful login. The real work begins once you uncover what actions were taken by the attacker.

## Step 5: Building a Bigger Picture
By now, you’ve probably realized that investigating brute force attacks follows a similar pattern, regardless of the protocol (SSH, RDP, etc.). This consistency is a blessing because once you master the steps, it becomes much easier and faster to analyze future attacks.

However, don’t underestimate the importance of understanding the big picture — what happens after an attacker gains access, and how can you spot it quickly? These are the questions that separate a basic investigation from a comprehensive one.

## Step 6: Automated Ticket Creation in OS Ticket
Now that we’ve completed the investigation, you’ll notice that the RDP alert we just investigated has already been routed to OS Ticket. This automatic ticket creation allows you to track and manage alerts more efficiently. Once in OS Ticket, you can assign the ticket to yourself or another team member for further review and action.

![Alt text](https://raw.githubusercontent.com/Virus192/Day-27-Investigating-RDP-Brute-Force-Alerts/refs/heads/main/Images/photo_6035328477617570253_w.jpg)

![Alt text](https://raw.githubusercontent.com/Virus192/Day-27-Investigating-RDP-Brute-Force-Alerts/refs/heads/main/Images/photo_6035328477617570252_w.jpg)


## Conclusion: Mastering Brute Force Investigations
Congratulations! You’ve now walked through the steps of investigating an RDP Brute Force attack and even automated your alert system with OS Ticket. As you’ve seen, the core methodology remains the same across protocols, and the more you practice, the quicker and more efficient your investigations will become.

In my next blog, we’ll dive deeper into the world of Mythic C2 — a powerful tool for simulating and investigating advanced attacks. We’ll analyze the activity generated by a Mythic C2 agent, giving you further insights into what to look for during a post-attack investigation.

Stay tuned — there’s much more to uncover in the exciting world of cybersecurity!
