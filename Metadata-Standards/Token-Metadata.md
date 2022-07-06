---
layout: default
title: Token Metadata
nav_order: 5
parent: Metadata Standards
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
  "name": "Epic certificate",
  "description": "This is an amazing certificate!", 
  "image": "https://cdn.pixabay.com/photo/2020/07/01/14/30/zebra-5359826_1280.jpg", 
  "issuedby": "A distinguished institution",
  "external_url": "https://zerti.com.ar", 
  "attributes": [
      {
          "trait_name": "Year", 
          "trait_value": "2022"
      },
      {
          "trait_name": "Honors", 
          "value": "true"
      }
      {
          "trait_name": "Extracurricular activities",
          "value":["Theatre Club", "Debate Club" , "Voleyball Club"]
      }
  ]
}
```