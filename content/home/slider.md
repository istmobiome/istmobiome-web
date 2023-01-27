---
widget: slider  # Use the Slider widget as this page section
weight: 1  # Position of this section on the page
active: true  # Publish this section?
headless: true  # This file represents a page section.

design:
  # Slide height is automatic unless you force a specific height (e.g. '400px')
  slide_height: '300px' #40vh
  is_fullscreen: false
  # Automatically transition through slides?
  loop: true
  # Duration of transition between slides (in ms)
  interval: 0
content:
  slides:
    - title: <small>The</small> Istmobiome <small>Project</small>
      content: a microbial tale told in two oceans 
      align: center
      background:
        position: right
        color: '#1f4e74FF'
        brightness: 1.0
        #media: coders.jpg
        #fit: cover
    - title: ''
      content: The rise of the Panama Isthmus changed the world. Millions of years ago an experiment began...
      align: center
      background:
        position: center
        color: '#1f4e74FF'
        brightness: 1.0
        #media: contact.jpg
        #fit: cover
    - title: ...on Land...
      content: it connected two continents
      align: right
      background:
        position: center
        color: '#333'
        brightness: 0.8
        media: site_banner/land.jpg
        fit: cover
      #link:
        #icon: graduation-cap
        #icon_pack: fas
        #text: Join Us
        #url: ../contact/
    - title: ...in the Sea.
      content: it divided one ocean
      align: right
      background:
        position: center
        color: '#333'
        brightness: 0.8
        media: site_banner/sea.jpg
        fit: cover
    - title: ''
      content: Click here to learn more about the project.
      align: center
      link:
        icon: info
        icon_pack: fas
        text: More Information
        url: about/
      background:
        position: center
        color: '#1f4e74FF'
        brightness: 0.5
---
