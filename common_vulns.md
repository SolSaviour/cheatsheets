# TryHackMe Notes

To make collapsible sections:

```html
<details>
    <summary>Click to Expand!</summary>
    <!-- Leave empty line here -->
    # Put content
</details>
<!-- Leave empty line here -->
```

## Index

* [XXE](#xxe)

---

## Room: [OWASP Top 10](https://tryhackme.com/room/owasptop10)

### Broken Authentication

You can register as " username" (notice the space at the start) and then log in with " username" and the password you set, and if authentication isn't configured properly you can access the account belonging to "username".

### XXE

XXE (eXtensible XML Entity) abuses features of XML parsers. E.g.

```xml
<?xml version="1.0"?>
<!DOCTYPE root [<!ENTITY read SYSTEM 'file:///etc/passwd'>]>
<root>&read;</root>
```

There are two types of XXE:

1. In-band XXE - the attacker can receive a response immediately.
2. Out-of-band XXE (OOB-XXE) - aka "Blind XXE" is where the attacker must reflect the output of the XXE payload.

**DTD** - Document Type Definition: Essentially, it is a schema (like in SQL, "DDL"). E.g.

```xml
<!DOCTYPE note [ <!ELEMENT note (to,from)> <!ELEMENT to (#PCDATA)> <!ELEMENT from (#PCDATA)> ]>
```

> Note: #PCDATA means parseable character data.

### IDOR

IDOR (Insecure Direct Object Reference) exploits misconfiguration of user input handling. I.e. Not checking if a user is authenticated to access `https://example.com/bank?account_number=1` instead of `https://example.com/bank?account_number=2`.

---

## Room [Upload Vulnerabilities](https://tryhackme.com/room/uploadvulns#)

### Filtering

There are two ways of filtering files before allowing them to be uploaded. 

1. Client-side filtering (extremely weak)
2. Server-side filtering

The main methods of filtering include:

* Extension Validation - Checking the file extension (easy to bypass).
* File Type Filtering - Using either file magic numbers of MIME types.
* File Length Filtering - Somewhat annoying but not impossible to bypass.
* File Name Filtering - Well administered systems will often sanitise filenames before upload to remove bad characters. 
* File Content Filtering - Check file content in order to identify and assess risk.

If an application employs client-side filtering, here are some ways to bypass it:

1. Turn off your browsers JS
2. Using Burp, intercept and modify the incoming page, stripping out the JS filter
3. Intercept and modify the file upload after it has gone through filtering
4. Send the file directly to the upload point

**Regarding number 2:** "Burpsuite will not, by default, intercept any external Javascript files that the web page is loading. If you need to edit a script which is not inside the main page being loaded, you'll need to go to the "Options" tab at the top of the Burpsuite window, then under the "Intercept Client Requests" section, edit the condition of the first line to remove `^js$|`".

**Regarding number 3:** You can simply change the file extension of the file you want to upload to match one in the whitelist, then intercept the REQUEST and change the MIME type to the intended MIME type of the file (so that the server-side knows how to execute it) and change the file extension back to the original.


