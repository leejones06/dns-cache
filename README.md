<h1> An Introduction to DNS </h1>

<p><i>Note: this project uses the VMs and network created in the previous projects.</i>i></p>

<h2> Part 1: Explore the Local DNS Cache </h2>

<b>Step 1 – Locate the DNS Cache</b>

-	Log into the Client VM as the administrator (mydomain.com/miles_dyson). 
-	Open PowerShell as the admin and ping “mainframe”.
-	
<img width="681" height="474" alt="image" src="https://github.com/user-attachments/assets/61b82415-07da-49f4-9ce4-d9ce26492f6e" />
</br>
</br>
<img width="681" height="296" alt="image" src="https://github.com/user-attachments/assets/49fb84f6-c8f2-4c01-9e8e-2a8e67f7fcb6" />

- You’ll notice that it doesn’t find anything. That’s because there is no “mainframe” record on the network. We’re going to create one!
  
<p><b>But first, let’s get a little background on ping and what happens when it searches for the record that matches the request. </b></p>

<p>There’s an order to it’s search: first, it will look in the local DNS cache for the record. If it doesn’t find an answer, it next looks in the local host file. If there’s nothing there, it will then look to the DNS server. If it doesn’t find anything there, the search will fail. </p>

<p><i>Create a record for “mainframe”</i></p>

-	In PowerShell, run the following command: “ipconfig /displaydns”. This will show you the <i>local DNS cache</i>. 

<img width="681" height="350" alt="image" src="https://github.com/user-attachments/assets/2dc92d00-5f4e-4850-b2ff-967524ac175d" />
</br>
</br>
<img width="681" height="587" alt="image" src="https://github.com/user-attachments/assets/14c930a0-9a0c-450e-a071-d24cb770b5e0" />

-	You can print it to a text file by running “ipconfig /displaydns > test.txt. Press “Enter”, then run “notepad test.txt”.

<img width="681" height="281" alt="image" src="https://github.com/user-attachments/assets/ff7859a3-82b7-40cd-b5c3-906942c9bc5f" />

<img width="681" height="502" alt="image" src="https://github.com/user-attachments/assets/44ed69d7-5e8b-4ea0-93e1-ed6ce2164a80" />

<b>Step 2 – Create a new record in the local host file. </b>

-	While still in the Client VM, open notepad as the administrator. 

<img width="681" height="502" alt="image" src="https://github.com/user-attachments/assets/416bc78d-f665-4416-a812-3eb34dc1e244" />

-	In Notepad, choose “open”, change to “all files”. In system32, open “drivers” folder, then etc, then the file names “hosts”. This is the local host file. You can use this file to map an IP address to a host name.

<img width="681" height="421" alt="image" src="https://github.com/user-attachments/assets/67a0592a-40ba-4ec6-9b74-418c3aa50c14" />

<img width="681" height="462" alt="image" src="https://github.com/user-attachments/assets/8785fe95-3895-457d-b25f-bf5061a58509" />

-	Go back to PowerShell and ping “Zebra” (this is a random name; it can be anything); it obviously doesn’t find anything. 
-	Go back to the “hosts” file in notepad. We’re going to assign the loopback address (127.0.0.1) to “zebra” (follow the image below).

<img width="681" height="492" alt="image" src="https://github.com/user-attachments/assets/34529bd9-3cf4-4f05-92e5-232751a832e9" />

-	Save the file and go back to PowerShell. Ping “zebra” and see what happens.

<img width="681" height="554" alt="image" src="https://github.com/user-attachments/assets/bb29697b-09ee-4f5b-a955-55e8b3de934b" />

<b>Step 3 – Next, we’ll create a DNS A-record. </b>
-	Go to the Server VM and open DNS Manager by going to the Windows Administrative Tools dropdown menu on the start button and select “DNS”. 

<img width="681" height="665" alt="image" src="https://github.com/user-attachments/assets/39b80bb6-49d7-4785-9e92-6a474d3ec657" />

-	The DNS menu will open. On the left side window, select “Forward Lookup Zones” under the Domain Controller. Then select “mydomain.com” You’ll see some A-records and their corresponding IP addresses (notice the private, static IP of the DC VM).
-	Right click in the records window and choose “new host…”

<img width="681" height="561" alt="image" src="https://github.com/user-attachments/assets/ecc03af7-ca69-4341-809e-fc693aa5ac96" />

-	In the “new host” window, put “mainframe” as the name; the private IP of the domain controller is 10.0.0.4; put that for the IP address; 
 

-	Now mainframe shows up in the records

 


<p><i>Notice we gave our new record the wrong IP. If you ping “mainframe”, it still won’t be found. To correct it, simply double-click on the mainframe record and change the IP. However, if you then go back and ping mainframe, you’ll see it still can’t find; it continues to show the incorrect IP address. What’s going on? The issue is that the local DNS cache doesn’t update right away. It still has my original incorrect IP entry associated with that record. Remember that the local cache is the first place ping looks for a record. Since it’s finding a record for mainframe (even though it’s incorrect), it stops it’s searching. We have to clear the DNS cache. So let’s do it…</i?</p>

<b>Step 4 –Flush the DNS Cache</b>
-	Go back to the Domain Controller and, in PowerShell, run the “ipconfig /flushdns” command. This will clear the local cache of non-host records; this includes our incorrect “mainframe” record. If you’d like to confirm this, run the “displaydns” command again, open the records in notepad, and search for “mainframe”. 

<img width="681" height="473" alt="image" src="https://github.com/user-attachments/assets/1df419f6-4041-444d-a8a1-388f61d5213e" />

-	Now you can ping “mainframe” and it can be found.

<img width="681" height="649" alt="image" src="https://github.com/user-attachments/assets/a8adc930-3bd5-4dc6-91dc-98bf946b9c7d" />

<p><b>This has been just a brief introduction to the DNS cache and how to use the ping command. </b></p>
