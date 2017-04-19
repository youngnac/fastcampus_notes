[template link](https://docs.djangoproject.com/en/1.10/topics/templates/)

# Templates
> convenient way to generate HTML 

> Django defines a standard API for loading and rendering templates regardless of the backend.

## Support for Template Engines
### Configuration
- template engines are configured with TEMPLATES setting
- in our case: 

```python
TEMPLATES_DIR = os.path.join(BASE_DIR, 'templates')
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': [
            # templates folder root 설정
            TEMPLATES_DIR
        ],
        ...
```

##### ??Context Processor 
- [link](https://docs.djangoproject.com/en/1.10/ref/templates/api/# context-processors)

