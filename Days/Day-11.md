## Day 11: What is a Brute Force Attack?

### 1. Attack Definition and Goal
• **Goal**: To gain unauthorized access to an account by systematically trying every possible credential combination.  
• **Method**: This attack is highly automated, targeting services like RDP and SSH that are exposed to the internet.

### 2. Attack Subsets
• **Simple Brute Force**: Sequential trial-and-error, attempting every possible character and digit permutation.  
• **Dictionary Attack**: Uses pre-compiled word lists containing common words and passwords sourced from credential dumps (leaked credentials). This has a higher success rate than simple brute force.  
• **Credential Stuffing**: Uses leaked username/password pairs from one breach and tries those combinations against multiple different services or websites.

### 3. Protection and Defense
• **Long Passwords/Passphrases**: Use passwords that are 15 or more characters long and unique. Passphrases are often easier to remember and harder to guess than complex passwords.  
• **Multi-Factor Authentication (MFA)**: Provides a critical secondary defense layer. MFA should be utilized, with authenticator apps preferred over SMS or email methods.  
• **Attack Surface Reduction**: Enterprises must disable or firewall public-facing services (RDP, SSH) if they are not essential.  
• **Vigilance**: Maintain awareness of phishing attempts and monitor personal accounts against public credential dumps.

### 4. Common Attack Tools
• Hydra  
• Hashcat  
• John the Ripper
