# Styling Guidelines
- File may start with metadata in JSON format (to be implemented)
- Article must start with a H1 (# Heading)
    - This will be used as the article's title
- Article may not contain more than one H1
- Use headings in order (H1 > H2 > H3)
- Do not add additional line breaks (\<br>) in front of headings. The markdown renderer will add padding anyways

## Meta
Example Meta:
```json
{
    // If set, will redirect to the path specified, ignoring any other content and meta in this file
    "mirrorOf": "../README.md", 
    
    // Overrides the page title (normally generated from frist heading) 
    "title": "Styling Guide"    
}
```