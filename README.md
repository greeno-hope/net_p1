# networks-p1
Template for networks portfolio exercise

## Introduction
Networks Portfolio-1 is an exercise in writing code that uses a networking protocol (in this case the email transfer protocol SMTP) in order to accomplish some task, i.e. sending email.  

Most of the work is already done for you you just have to write some code to send some SMTP commands (with appropriate arguments and appropriately formatted) in order to send ***at least one email*** from ***the starting script*** which you will modify.

You are provided with a starting file, smtpstarter.py. This file is a python SMTP client your task in this portfolio is to complete the missing functionality and send (at least) one test email successfully. You will principally be working on the code between lines 73 and 104 but you will also have to make small changes in the send_one_email() function at the bottom.

***You will have to*** create a valid (free) outlook account in order to run the script as it is written (*the gmail settings on your Hope email accounts mean this script will not work using your university email logins*), although you are welcome to extend the script to use other authentication mechanisms and/or servers if you want to earn brownie points. 

You should review slides 19-26 from the week 4 networks lecture 'The Application Layer (part B)' to remind yourself how the basic SMTP protocol works. The script here is slightly more complicated that the flow shown in slide 25 because it also handles encryption and authentication but you will be implementing only simple SMTP commands.  

## The starting code

### Lines 1-70

These lines are your foundation and you should not need to change anything here. The functions are:

***Utility functions***
- send_line() sends a string of text to the SMTP server (printing the string sent)
- getError() parses the (int) error from a server reply string

***SMTP protocol functions*** are used in the oder they are declared here
- connect() opens a client side TCP socket on port 586 (the port that outlooks SMTP servers listen on for incoming connections that will swp to TLS
- send_ehlo() sends a ***EHLO*** command to the server (which retrieves a list of protocols supported by this SMTP server)
- start_tls() sends a ***STARTTLS*** command and then wraps the client_socket in a TLS handler
- send_helo() sends a traditional ***HELO*** command
- send_auth_request() sends a ***AUTH LOGIN*** command. This tells the server that you want to authenticate using basic username/password authentication
- send_username() sends your username (username ***base64 encoded***)
- send_password() sends your password (password ***base64 encoded***)

### Lines 73-104
***This is the code that you will have to complete in order to send an email***.  

Here you will see ***TODO*** comments in 5 functions these are the functions that you will need to complete the implementation for

- send_mail_from() This function should send and handle a ***MAIL FROM*** command 
- send_rcpt_to() This function should send and handle a ***RCPT TO*** command
- send_begin_data() This function should send and handle a ***DATA*** command
- send_message() This function should send a message body optionally witha ***SUBJECT*** line.
- send_quit() This function should send and handle a ***QUIT*** command

You should refer to the earlier send_line() and send_helo() function  to see how commands are formatted and sent.   
Also refer to the log output at the bottom of this page to see the strings that are send/recived in a completely working client. 

### After line 104

This section contains the entry point 

    if __name__ == '__main__'
        send_one_email()
        
The send_one_email() function drives everything and you should only have to some values here to run the basic test script
- YOUR_OUTLOOK_EMAIL_ADDRESS_HERE needs to be ***your*** outlook email address
- YOUR_OUTLOOK_PASSWORD_HERE needs to be ***your*** password and
- TARGET_EMAIL_HERE seeds to be a valid email address

## Marking

- Implementing the code such that you can send one test email will get a pass ~50%
- Adding appropriate error checking and reporting errors to the user will earn up to 75%
- Modifying the code such that you can send several messages ***from keyboard input*** will get close to 100%

## Python print statements from working code
The output below is from the fully wortking SMTP client 


CLIENT: Connecting to smtp-mail.outlook.com   

SERVER: 220 LO2P265CA0443.outlook.office365.com Microsoft ESMTP MAIL Service ready at Thu, 17 Nov 2022 13:42:50 +0000

CLIENT: ehlo smtp-mail.outlook.com   

SERVER: 250-LO2P265CA0443.outlook.office365.com Hello [194.81.33.11]
250-SIZE 157286400
250-PIPELINING
250-DSN
250-ENHANCEDSTATUSCODES
250-STARTTLS
250-8BITMIME
250-BINARYMIME
250-CHUNKING
250 SMTPUTF8

CLIENT: STARTTLS  

SERVER: 220 2.0.0 SMTP server ready

CLIENT: helo smtp-mail.outlook.com   

SERVER: 250 LO2P265CA0443.outlook.office365.com Hello [194.81.33.11]

CLIENT: auth login.  

SERVER: 334 VXNlcm5hbWU6

CLIENT: XXXXXXXXXXXXXXXXXXXXXXXXXXX   

SERVER: 334 UGFzc3dvcmQ6

CLIENT: XXXXXXXXXXXXXXXXXXXXXXXXXXX  

SERVER: 235 2.7.0 Authentication successful

CLIENT: mail from: <my_email_address@outlook.com>  

SERVER: 250 2.1.0 Sender OK

CLIENT: rcpt to: <greenwm1@hope.ac.uk>  

SERVER: 250 2.1.5 Recipient OK

CLIENT: data  

SERVER: 354 Start mail input; end with <CRLF>.<CRLF>

CLIENT: subject: Nothing much  

CLIENT: Hello World  
 
CLIENT:   

.   


SERVER: 250 2.0.0 OK <DBBPR03MB5414097781BC157026369C79A4069@DBBPR03MB5414.eurprd03.prod.outlook.com> [Hostname=DBBPR03MB5414.eurprd03.prod.outlook.com]

CLIENT: quit  

SERVER: 221 2.0.0 Service closing transmission channel
