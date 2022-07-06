---
layout: default
title: Token Metadata
nav_order: 5
---

# Token Metadata
{: .no_toc }

<details open markdown="block">
  <summary>
    Table of contents
  </summary>
  {: .text-delta }
1. TOC
{:toc}
</details>

---
## Our objective
We seek to provide a variable and modular .json metadata standard, to accomodate for the different types of certifications that exist and that may be emmited.
## Example

```json
{
  "name": "Zerti",
  "description": "This is certificate from the Zerti app", 
  "image": "https://storage.googleapis.com/opensea-prod.appspot.com/puffs/3.png", 
  "external_url": "https://zerti.com.ar", 
  "attributes": [
      {
          "trait_type": "Year", 
          "value": "2022"
      },
      {
          "trait_type": "Year", 
          "value": "2022"
      }
  ]
}
```