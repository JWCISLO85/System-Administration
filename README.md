𝐒𝐞𝐭𝐭𝐢𝐧𝐠 𝐔𝐩 𝐌𝐢𝐜𝐫𝐨𝐬𝐨𝐟𝐭 𝐒𝐞𝐫𝐯𝐞𝐫 𝐃𝐇𝐂𝐏 𝐅𝐚𝐢𝐥𝐨𝐯𝐞𝐫
This week I completed a lab on configuring DHCP failover between two Windows Server 2019 domain controllers. I did run into some difficulties which gave me the opportunity to use some real-world troubleshooting.

𝐖𝐡𝐚𝐭 𝐈 𝐀𝐜𝐜𝐨𝐦𝐩𝐥𝐢𝐬𝐡𝐞𝐝
• Configured Server Manager to manage multiple servers remotely
• Installed and configured DHCP role with custom scope
• Set up IP exclusions and lease durations
• Successfully activated the scope and verified client IP assignment

𝐅𝐚𝐢𝐥𝐨𝐯𝐞𝐫 𝐂𝐨𝐧𝐟𝐢𝐠𝐮𝐫𝐚𝐭𝐢𝐨𝐧
• Implemented Hot Standby mode between two domain controllers
• Configured partner server relationships with shared secrets
• Set up maximum client lead time and standby server address allocation

𝐓𝐫𝐨𝐮𝐛𝐥𝐞𝐬𝐡𝐨𝐨𝐭𝐢𝐧𝐠
I did come across some challenges during this lab but this was a valuable learning experience for me:

𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞#1 𝐂𝐨𝐦𝐩𝐞𝐭𝐢𝐧𝐠 𝐃𝐇𝐂𝐏 𝐒𝐞𝐫𝐯𝐞𝐫𝐬 My Windows 10 client kept getting IP addresses I had excluded. After investigating, I discovered the firewall/router had its on DHCP server running. I had to access the M0n0wall web GUI to disable the competing service. This is a good reminder to check all devices and not just one that you are working on.

𝐂𝐡𝐚𝐥𝐥𝐞𝐧𝐠𝐞#2 𝐃𝐨𝐦𝐚𝐢𝐧 𝐂𝐨𝐧𝐟𝐢𝐠𝐮𝐫𝐚𝐭𝐢𝐨𝐧 𝐌𝐢𝐬𝐭𝐚𝐤𝐞 The biggest learning moment came trying to set up failover between servers. I had accidently created a new forest instead of adding the second server to the existing domain. This prevented the failover configuration entirely and required rebuilding the virtual machine.

𝐊𝐞𝐲 𝐓𝐚𝐤𝐞𝐚𝐰𝐚𝐲𝐬
1. 𝐃𝐨𝐜𝐮𝐦𝐞𝐧𝐭𝐚𝐭𝐢𝐨𝐧 𝐢𝐬 𝐜𝐫𝐮𝐜𝐢𝐚𝐥-Following procedures exactly prevents costly misconfigurations
2. 𝐍𝐞𝐭𝐰𝐨𝐫𝐤 𝐭𝐫𝐨𝐮𝐛𝐥𝐞𝐬𝐡𝐨𝐨𝐭𝐢𝐧𝐠 𝐬𝐤𝐢𝐥𝐥𝐬 𝐚𝐫𝐞 𝐞𝐬𝐬𝐞𝐧𝐭𝐢𝐚𝐥-Used ping tests, DNS configuration updates , and PowerShell commands
3. 𝐔𝐧𝐝𝐞𝐫𝐬𝐭𝐚𝐧𝐝𝐢𝐧𝐠 𝐭𝐡𝐞 𝐝𝐢𝐟𝐟𝐞𝐫𝐞𝐧𝐜𝐞 𝐛𝐞𝐭𝐰𝐞𝐞𝐧 𝐇𝐨𝐭 𝐒𝐭𝐚𝐧𝐝𝐛𝐲 𝐚𝐧𝐝 𝐋𝐨𝐚𝐝 𝐁𝐚𝐥𝐚𝐧𝐜𝐢𝐧𝐠-Hot Standby keeps one server as primary with automatic failover
4. 𝐖𝐢𝐧𝐝𝐨𝐰𝐬 𝐮𝐬𝐞𝐬 𝐆𝐫𝐨𝐮𝐩 𝐏𝐨𝐥𝐢𝐜𝐲 𝐚𝐧𝐝 𝐑𝐞𝐠𝐢𝐬𝐭𝐫𝐲 for maintaining configurations between servers different to Linux which uses configuration files

𝐓𝐞𝐜𝐡𝐧𝐢𝐜𝐚𝐥 𝐃𝐞𝐭𝐚𝐢𝐥𝐬 𝐋𝐞𝐚𝐫𝐧𝐞𝐝
𝐌𝐚𝐱𝐢𝐦𝐮𝐦 𝐂𝐥𝐢𝐞𝐧𝐭 𝐋𝐞𝐚𝐝 𝐓𝐢𝐦𝐞: The period a server can extend a lease beyond what a partner server knows
𝐀𝐝𝐝𝐫𝐞𝐬𝐬𝐞𝐬 𝐑𝐞𝐬𝐞𝐫𝐯𝐞𝐝 𝐟𝐨𝐫 𝐒𝐭𝐚𝐧𝐝𝐛𝐲: Percentage of IP range allocated to standby server during failover
𝐒𝐞𝐚𝐦𝐥𝐞𝐬𝐬 𝐟𝐚𝐢𝐥𝐨𝐯𝐞𝐫: When properly configured users shouldn't notice when the primary server goes down for maintenance

𝐓𝐨𝐨𝐥𝐬 𝐔𝐬𝐞𝐝
• Windows Server 2019(Domain Controllers)
• Windows 10(Client testing)
• Server Manager & DHCP Management Console
• PowerShell(ipconfig commands)
• Mon0wall firewall management
