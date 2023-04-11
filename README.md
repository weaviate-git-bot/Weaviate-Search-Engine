# Weaviate Image Search Engine
Vector database (Weaviate) to build an image search engine powered by a deep neural network. It uses Weaviate optimizer for images with **ResNet50 (PyTorch)** as vectorizer and retriever. Currently only PyTorch ResNet50 supports **CUDA-GPU** but in a **single-threaded** manner. You can also use the **keras-based ResNet50 CPU** with **multi-threaded** inference.

## Steps to run it
1. Install Docker (skip this if already installed)
```sh
curl -fsSL https://get.docker.com -o get-docker.sh && sh get-docker.sh
```

2. Spin up docker compose file:
```sh
docker compose up -d
```

3. Install `requirements.txt`:
```sh
pip3 install -r requirements.txt
```

4. Launch `start.sh` script to create Weaviate schema, convert images from `img` folder to base64 and upload them in Weaviate:
```sh
./start.sh
```

5. Navigate to URL that streamlit outputs and upload an image. The program will query Weaviate for a vectorized search of similar images. If you are running this in local environment then just navigate at:
```
http://127.0.0.1:8501/
```

## Images
Feel free to insert your images in `img` folder and see if the database suggests you similar images.

## Weaviate Schema
This is the schema implemented in Weaviate. You can modify the class name, change the vectorizer or insert more properties.
```python
schemaConfig = {
    'class': 'MyImages', # class name for schema config in Weaviate (change it with a custom name for your images)
    'vectorizer': 'img2vec-neural',
    'vectorIndexType': 'hnsw',
    'moduleConfig': {
        'img2vec-neural': {
            'imageFields': [
                'image'
            ]
        }
    },
    'properties': [
        {
            'name': 'image',
            'dataType': ['blob']
        },
        {
            'name': 'text',
            'dataType': ['string']
        }
    ]
}
```
