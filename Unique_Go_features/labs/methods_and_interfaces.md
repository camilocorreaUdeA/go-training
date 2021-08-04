# Time to practice...

Build a scraper or Web Crawler from scratch.

For this challenge we need to get information on some packages dispatched by the following courier companies

* servientega: 2023964550
* redservi: 300000817085

Use the following interface for each scraper and return the data with the following structure 

Interface
```golang
type TrackingDetails struct {
	// TODO: Add fields
}

type Fetcher interface {
    Fetch(trackingNumber string, client http.Client) (*TrackingDetails, error)
}
```

Output
```json
"tracking_details": [
{
    "message": "Guia generada",
    "datetime": "2015-12-31T15:58:00Z",
    "tracking_location": {
        "city": "Medellin",
        "state": "Antioquia",
    }
}, 
{
    "message": "Ingreso al centro logistico",
    "datetime": "2015-12-31T15:58:00Z",
    "tracking_location": {
        "city": "Medellin",
        "state": "Antioquia",
    }
}]
```

* https://golang.org/pkg/net/http/
* https://github.com/PuerkitoBio/goquery
* https://github.com/jarcoal/httpmock