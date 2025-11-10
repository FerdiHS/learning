# Lecture 05: DNS (Domain Name System)

## DNS Components

- An **hierarchical name system**
- **Distributed database**
- **Application** on top of UDP/TCP
    - DNS Resolver (Client)
    - DNS Server application
    - DNS server port 53

---

## Top-Level Domain (TLD) servers

- Responsible for *com*, *org*, *net*, *edu*, *aero*, *jobs*, *museums*, and all top-level country domains, such as *uk*, *fr*, *ca*, *jp*
- Network Solutions maintains server for *.com* TLD
- Educause for *.edu* TLD

## Authoritatives DNS servers

- Organization’s own DNS server(s)
- Providing authoritative hostname to IP mappings for organization’s named hosts
- Can be maintained by organization or service provider

## Local DNS Server

- Does not strictly belong to hierarchy
- Each ISP has one (also called “default name server”)
- Has a local **cache**
- Acts as **proxy**, forwards query into hierarchy

---

## DNS Name Resolution

### Iterated Query

- Contacted server replies with name of server to contact
- Local DNS contact server iteratively

### Recursive Query

- Puts burden of name resolution on contacted name server
- All queried servers will know the name resolution

---

## Domain

a subtree of the domain name space

## Zone

what a server is responsible for or has authority over

---

## Primary Server

- Stores information about zone it is an authority for
- Creates, maintains and updates zone file

## Secondary Server

- Has the complete information about a zone
- Cannot create or update zone file

Both provide authoritative answer (guaranteed to be accurate) for their zone

---

## Resource Records

Format: `(name, value, type, til)`

- `type` = A (Address for the canonical names)
    - `name` is hostname
    - `value` is IP address
- `type` = NS (List a name server for this domain)
    - `name` is domain (e.g. [*foo.com*](http://foo.com))
    - `value` is hostname of authoritative name server (primary and secondary) for this domain
- `type` = CNAME (Alias)
    - `name` is alias name for some canonical name (e.g. [*mgnzsqc.x.incapdns.net*](http://mgnzsqc.x.incapdns.net) instead of [*www.nus.edu.sg*](http://www.nus.edu.sg))
    - `value` is canonical name
- `type` = MX (List a mail server for this domain)
    - `value` is the name of mail server associated with `name`
- `type` = SOA (Indicates authority for this domain data)
    - `name` is domain
    - `value` is hostname of authoritative name server (only primary) for this domain and other information
    - `ttl` is the *time-to-live* when cached by others
- `type` = PTR (Pointer address-to-name mappings)
    - `name` is IP address
    - `value` is host name

---

## DNS Messages Format

- Query: `Header | Question Section`
- Response: `Header | Question Section | Answer Section | Authoritative Section | Additional Section`
- Header: `Identification | Flags | Number of Question Records | Number of Answer Records | Number of Authoritative Records | Number of Additional Records`
- Flags: `QR | OpCode | AA | TC | RD | RA | Three 0s | rCode`
    - QR: 0 for query, 1 for response
    - OpCode: standard (0), inverse (1), server status (2)
    - AA: 1 if server is authoritative
    - TC: 1 if truncated to 512 (if UDP is used)
    - RD: 1 if client desires recursive answer
    - RA: 1 if recursive response is available
    - rCode: Error Code
        - 0: No error
        - 1: Format error
        - 2: Problem at name server
        - 3: Domain reference problem
        - 4: Query type not supported
        - 5: Administratively prohibited
        - 6-15: Reserved
- Question Record: `Query Name | Query Type | Query Class`
    - Query Class
        
        
        | Class | Mnemonic |
        | --- | --- |
        | 1 | Internet |
        | 2 | CSNET |
        | 3 | CS |
        | 4 | HS |
    - Query Types
        
        
        | Type | Mnemonic | Description |
        | --- | --- | --- |
        | 1 | A | **Address**. A 32-bit IPv4 address. |
        | 2 | NS | **Name server**. Identifies the authoritative servers for a zone. |
        | 5 | CNAME | **Canonical name**. |
        | 6 | SOA | **Start of authority**. |
        | 11 | WKS | **Well-known services**. The network services that a host provides |
        | 12 | PTR | **Pointer**. Used to convert IP address to domain name. |
        | 13 | HINFO | **Host information**. Defines the hardware and OS. |
        | 15 | MX | **Mail exchange**. Redirects mail to a mail server. |
        | 28 | AAAA | **Address**. ****An IPv6 address |
        | 252 | AXFR | A request for the transfer of the entire zone |
        | 255 | ANY | A request for all record |
- Resource Record Format: `Domain name | Domain type | Domain class | Time to live | Resource data length | Resource data`