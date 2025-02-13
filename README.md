```python 
@dlt.resource(name="rides", write_disposition='append')
def ny_taxi() -> Generator[list[dict[str, Any]], Any, Any]:
    client = RESTClient(
        base_url="https://us-central1-dlthub-analytics.cloudfunctions.net",
        paginator=PageNumberPaginator(
            base_page=1,
            total_path=None
        )
    )

    for page in client.paginate("data_engineering_zoomcamp_api"):
        yield page
```