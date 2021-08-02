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
