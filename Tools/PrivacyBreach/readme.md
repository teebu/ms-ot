Sample text with multiple regions:

```
There was a breach in Azure SQL containing 5000 credit card numbers of Data Subjects from France, Germany and California. Who do I need to notify?
```

```
There was a breach in Azure SQL containing 5000 credit card numbers of Data Subjects from France. Who do I need to notify?
```

payload:
```
{
  "name": "Credit Card Data Breach",
  "description": "A data breach involving 5000 personal records containing credit card numbers has occurred, affecting individuals in France, Germany, and California.",
  "jurisdictions": "France, Germany, California"
}
```
