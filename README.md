# NerdyADLab

## First off GitHub Noob Alert

This is my first go at this and I will try my best to figure it out. First off thanks to Carl Webster, Martin Zugec and Guy Leech for their help with the PowerShell help. I have had this idea for a while, but as I was working on my books AD deployment, I wanted it to be more fun so I made this detour to try and do what I could to help out others.

Secondly, I hope people enjoy this project and it finds a spot in domains all over the world. I hope that it will be become even better than what it is now. I’m not a super PowerShell nerd, but I use Excel all the time and that is why built this using Excel to make the PowerShell commands to hopefully make it easier for others who are not big coders.

### What in the world is this thing?

This is a Microsoft Excel workbook that uses the concatenate function along with list of names to make the commands needed to make your first OUs, Sub OUs, Groups and most importantly users.  This will make fun users to make things a little more entertaining in demos and screenshots and doing work in a test domain. I don’t think it is fun to just add a couple generic accounts most of us in the IT field are fans of Science Fiction and I wanted to pick a couple of my favorites to add them to the NerdyADLab project.

## Description

This is my first go at this and I will try my best to figure it out. First off thanks to Carl Webster, Martin Zugec and Guy Leech for their help with the PowerShell help. I have had this idea for a while, but as I was working on my books AD deployment, I wanted it to be more fun so I made this detour to try and do what I could to help out others.

Secondly, I hope people enjoy this project and it finds a spot in domains all over the world. I hope that it will be become even better than what it is now. I’m not a super PowerShell nerd, but I use Excel all the time which is why this was built using Excel to make the PowerShell commands to hopefully make it easier for others who are not big coders.

Table of Contents
Overview
How to Use It
How to Modify It

## Installation

No installation required. You just need a total of three things to get started.

1.	You need to have Microsoft Excel, Libra Office or Open Office to open this file and work with it.
2.	Then you need Active Directory (Azure or Local)
3.	Finally access to Microsoft PowerShell along with its accompanying Active Directory Module too.

How to Install the AD PowerShell Module from a Server OS 
Import-Module ServerManager Add-WindowsFeature -Name "RSAT-AD-PowerShell" –IncludeAllSubFeature
How to Install the AD PowerShell Module from a Desktop OS 
Enable-WindowsOptionalFeature -Online -FeatureName RSATClient-Roles-AD-Powershell

## Usage

Make sure you have your domain ready to roll with your PowerShell Module and you are ready to shake and bake. The basic premise of this project is use Excel to make some PowerShell commands that you can easily past into your command prompt and build things out in AD. This can be customized to your deployment to make setting up a new lab that much easier.

I wanted to make this project as easy for people to take what they want and leave what they don’t. You can use this to build a simple Mock AD setup or just use it to add some fun users to into your existing domain. I wanted to also make sure it worked great for new deployments along with existing deployments so you can just edit a couple fields and you’re ready to past some characters into your domain.

Excel Function Detour
If you’re not an Excel Nerd I have tried to make this easy as possible without doing forms, macros or VB Script. We are only using 4 functions in this whole workbook and I want to cover their basics so you can know how it works a bit more. All Excel functions are the name of the function then a pair of parentheses () to host the contents. The cool thing I learned a long time ago is functions can go into functions which is where the fun can start. Just like any coding of anything opening and closing statements is critical and in Excel there is another little thing that can cause drama and that is the quotation marks “ because when you enter text into function it must be surrounded by them or it will break the function. () and “” are your friend and enemy all at once.

Concatenate
This takes User Entered Text and Items in a Cell and glues them together in a single cell for whatever you need.

```
Concatenate(A1,” This is the way ”,B1)
```

This will take the contents of A1 and B1 and put This is fun with spaces before and after in the middle. This is a great way to take say a list with First and Last name and make a Full Name in one column and many other items. This is the MVP function of this whole project because it is all centered on this concept, enter parts of the command here and there and then assemble the command to be able to paste into your deployment.

Concatenate Pro Tip, you cannot have any quotation marks “ in your text entries so if you need to add one you have to do it on its own.  

Example 

```
Concatenate(A1,””””,” This is the way ”,””””,B1) 
```

This will now output the same thing but now there will be quotes around “This is the way”.
I think about 15 years ago I ran into that problem and it took me forever to figure out how to make it happen. We had to do this in this file multiple times to be ready for spaces and based on the syntax requirements of the command.

IF
This will check a condition and then put what you specify for if it is true of false.

Example

```
IF(A1=”This is the way”,”Not the way”,”I have spoken”)
```

This will look at A1 and see if the text is “This is the way” and if it is not (False) then it will return “Not the way” and if it is there (True) then it will return “I have spoken”. You can use math in this function too which can be fun too. In this project this is used to read the “Yes” and “No” in the 1-StartHere worksheet and either enter something or not like the Password, Account being enabled and also IF is used to see if something is blank or not so you don’t see 42 lines of empty partial commands.

Example 

```
IF(A5="","",(Concatenate(A1,” This is the way ”,B1))
```

This is where we don’t concatenate unless A5 has a value in it.

Links – Each worksheet builds off the previous and I have noted all the things that are linked so if you want the Account OU to be named something different or you want to put your characters in a different OU you can just edit those linked cells. I have highlighted those cells in Yellow along with some text to the right of it to help you spot those.
![RootOUs](/_images/image2.png)
![SubOUs](/_images/image1.png)

Those are the big two that are running Barter Town for this project. They have just become pretty complicated because we have one big IF to check on if things are blank and then things will get concatenated or not based on multiple IFs within the concatenate too.

The Account Creation is the most complicated of all the things and its formula ends up looking like this. If you look at this, you can see things got a little wild with a combination of IFs with linked cells and entered text.

Pro Tip: When you see the $ sign in cell names $B$4 (You can use F4 in most versions when a cell has been selected to do this.) that means it is an absolute reference so when you drag that formula any direction it will not increment the cell relative to its position. So, if you need to add more users, groups or OUs past row 42 then it will keep the right things referenced.

```
=IF(A114="","",(CONCATENATE(IF('1-StartHere'!$B$4="",," $Password = ConvertTo-SecureString -String "),IF('1-StartHere'!$B$4="",,""""),IF('1-StartHere'!$B$4="",,'1-StartHere'!$B$4),IF('1-StartHere'!$B$4="",,""""),IF('1-StartHere'!$B$4="",," -Force -AsPlainText; ")," New-ADUser -Name ","""",A114,""""," -Path ","""","OU=",'3-Sub-OUs'!$A$11,",OU=",'2-Root-OUs'!$A$2,",DC=",'1-StartHere'!$B$1,",DC=",'1-StartHere'!$B$2,""""," -Verbose"," -CannotChangePassword $True -ChangePasswordAtLogon $False -Enabled $True -PasswordNeverExpires $True"," -SAMAccountName ","""",E114,""""," -UserPrincipalName ","""",E114,"@",'1-StartHere'!$B$1,".",'1-StartHere'!$B$2,"""",IF('1-StartHere'!$B$4="",," -AccountPassword $Password")," -Description """,F114,"""",)))
```

## 1-StartHere

![RootOUs](/_images/image3.png)

Here are the main things to fill out on this page. You need to just enter your domain information, if you want to protect OUs from accidental deletion, what password you want them to use and if you want the account enabled and your ready for the next worksheets.

1.	Domain Name
a.	VDILOCKDOWNGUIDE
2.	Domain TLD
a.	LOCAL
3.	Protect OUs from Deletion
a.	Yes
b.	No
4.	Default UN Password
a.	P@SsW0rd!@12
b.	Note: If no password is entered the accounts will be disabled upon creation.
5.	Accounts Enabled
a.	Yes
b.	No

## 2-Root-OUs

![RootOUs](/_images/image5.png)

You may have your personal preferences on how you want your lab setup with Root OUs so this is where you can just enter the names of all those OUs, and you will have commands to the right of them to create them. 

1.	!Accounts
a.	Linked to Sub OUs
b.	Linked to Groups also
c.	Linked to Accounts too
i.	All accounts will use this path so you can just edit the name to something you work in that field since it linked.
ii.	If you want other OUs just keep typing below and the commands will appear on the right.
2.	!Desktops
3.	!Servers
4.	42 Rows Later, (Why 42? Because it is the answer to the Ultimate Question of Life, the Universe, and Everything)
a.	You can just type the names of the OUs you want in Column A and the command will show up on the right. It is just using an IF statement that if the column A is blank it will not do the concatenate.

## 3-Sub-OUs

![SubOUs](/_images/image6.png)

This is the next part of the process where some default OUs are being created underneath those Root OUs. This is where you will get the commands to make these OUs and/or rename the Subordinate OUs.

1.	Sub-OUs to be Placed Under !Accounts
a.	There are some Generic ones and then under those are the fun ones to place the characters under their name along or whatever you want. The ones that are highlighted with the current version are highlighted and noted.
2.	Sub-OUs to be Placed Under !Desktops
a.	There are only Generics ones for now under this section. Add whatever you want below to Row 42.
3.	Sub-OUs to be Placed Under !Servers
a.	There are only Generics ones for now under this section. Add whatever you want below to Row 42.
4.	Sub-OUs to be Placed Under !Servers and Under Prod and Test
a.	This is used by both OUs to make them the same as hopefully they may be in real life.
b.	There are only Generics ones for now under this section. Add whatever you want below to Row 42.
5.	Sub-OUs to be Placed Under IT
a.	Right now, this just has SVC under it for now for a couple service accounts from another worksheet.

## 4-Groups

![RootOUs](/_images/image6.png)

This is where there are some generic groups again mocked up. Just keep adding whatever you want below to Row 42. As soon as you enter something into Column A then the command will get built out in Column D.

## 5-Service-Accounts

![ServiceAccounts](/_images/image7.png)

This is where there are some generic accounts again mocked up. Just keep adding whatever you want below to Row 42. As soon as you enter something into Column A then the command will get built out in Column D.

## Account Type Tabs

![AccountTypes](/_images/image8.png)

Right now, there are 6 groups of accounts. Some of them have better descriptions and others based on what dataset I could pull in. This is where if you want to edit something or add another character group please contribute and submit the 5 columns. Just use the “Help-Us” worksheet and enter the Full Name, First Name, Middle, Last Name, Username and the Description for the user and submit it for an enhancement and we will get the file updated.

### Quick Video of Adding some Charecters
(/_images/AddUserAccounts-Default.gif)

All you need to do is select each worksheet Select column G and you can use CTRL\Command + Shift + Down Arrow to Select all non-empty cells and copy and paste them into your PowerShell prompt.  Repeat this process for whichever charecter sets you want.

### The Future

I hope first that people just think this is useful and fun.

### Next Maybes

- More Character Groups (20 Charecter Account Name Limit)
- More Sub OUs (Make it more Real)
- More Default Service Accounts (Make it more Real)
- Bad Privileged Account Management Switch (Make it more Real)
  - This would be to put some characters into Account Operators, Backup Operators, Domain Admin, Enterprise Admin and Schema groups so that if you assess the deployment you may find some interesting users that maybe shouldn’t be in those groups (May use just random values)
- Someday for Penetration Testing I will pull some breach data and use that to make some User Accounts with real passwords out in the wild so the deployment will become weaker with real user passwords being used.
- Who Knows, Thoughts?

### Contributing

If you would like to contribute to this project, please let us know. If you find some bugs or you want to add more characters or if you have ideas, please just make an issue or pull request.
