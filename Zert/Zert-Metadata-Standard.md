---
layout: default
title: Zert Metadata Standard
nav_order: 3
parent: Zert
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

We seek to provide a customizable .json metadata standard, to accomodate for the different types of certifications that exist and that may be emmited.

---
## Metadata Structure

### General Data

| Key              | Definition                                                                                                            |
|:-----------------|:----------------------------------------------------------------------------------------------------------------------|
| name             | Name of the certificate.                                                                                              |
| description      | Description of the certificate.                                                                                       | 
| image            | URL of the image of the certifcate.                                                                                   |
| external_url     | URL that will appear below the asset's image and will allow users to leave and view the item on your site.            | 
| background_color | Background color of the certificate. Must be a six-character hexadecimal without a pre-pended #.                      |
| attributes       | These are the attributes for the certificate. (see below).                                                            |

### Atributes

To make our certificates customizable, we also allow users to add custom "attributes" to the certificate metadata that add personalised information about each certification.


| Key          | Definition                                             |
|:-------------|:-------------------------------------------------------|
| trait_type   | Name of the trait                                      |
| value        | Value of the trait(can be an array of any type)        |
| display_type | Field indicating how you would like it to be displayed |

### Display Types

| trait_type  | Definition                                       | Image |
|:------------|:-------------------------------------------------|:------|
| date        | Will show as date                               |       |
| award       | Will show as award  |       |

### Example

```json
{
  "name": "Epic certificate",
  "description": "This is an amazing certificate!", 
  "image": "https://cdn.pixabay.com/photo/2020/07/01/14/30/zebra-5359826_1280.jpg",
  "external_url": "https://zerti.com.ar", 
  "attributes": [
      {
          "display_type": "date",
          "trait_type": "Year", 
          "value": "2022"
      },
      {
          "display_type": "award",
          "trait_type": "Graduated with honors", 
          "value": "true"
      }
      {
          "trait_type": "Extracurricular activities",
          "value": ["Theatre Club", "Debate Club" , "Voleyball Club"]
      }
  ]
}
```