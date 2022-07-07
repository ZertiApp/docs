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

---
## Metadate Structure
### **General Data**
| Key      | Definition |
| ----------- | ----------- |
| name      | the name of the certificate|
| description   | a description of the certificate|
| image | an URL of the image of the certifcate|
| external_url | This is the URL that will appear below the asset's image and will allow users to leave and view the item on your site


---


### **Atributes**
| Key      | Definition |
| ----------- | ----------- |
| trait_type |  is the name of the trait|
| value | is the value of the trait|
| dysplay_type| is a field indicating how you would like it to be displayed |

**Dysplay Types**
| trait_type | Definition | Image |
| ----------- | ----------- | ---|
| date | will show the date| |
| prize | will show the the prize of the certificate | |



### Example

```json
{
  "name": "Epic certificate",
  "description": "This is an amazing certificate!", 
  "image": "https://cdn.pixabay.com/photo/2020/07/01/14/30/zebra-5359826_1280.jpg",
  "external_url": "https://zerti.com.ar", 
  "attributes": [
      {
          "dysplay_type": "date",
          "trait_type": "Year", 
          "value": "2022"
      },
      {
          "dysplay_type": "prize",
          "trait_type": "Honors", 
          "value": "true"
      }
      {
          "trait_type": "Extracurricular activities",
          "value": ["Theatre Club", "Debate Club" , "Voleyball Club"]
      }
  ]
}
```