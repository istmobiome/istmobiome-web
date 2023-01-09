---
widget: pages
headless: true  # This file represents a page section.
active: true  # Activate this widget? true/false
weight: 10

# ... Put Your Section Options Here (title etc.) ...
title: Publications
subtitle: Use the radio buttons for quick access to publication material or click the title link for additional details. {{% callout note %}} Quickly discover relevant content by [filtering publications](/publication/).{{% /callout %}}


content:
  # Page type to display. E.g. post, talk, publication...
  # I THINK this is the name of the directory with content
  page_type: publication 
  # Choose how much pages you would like to display (0 = all pages)
  count: 0
  # Choose how many pages you would like to offset by
  offset: 0
  # Page order: descending (desc) or ascending (asc) date.
  order: desc
  # Filter on criteria
  filters:
    tag: ''
    category: ''
    publication_type: ''
    author: ''
    exclude_featured: true
design:
  # Choose a view for the listings:
  #   1 = List
  #   2 = Compact
  #   3 = Card
  #   4 = Citation (publication only)
  view: 3
---
