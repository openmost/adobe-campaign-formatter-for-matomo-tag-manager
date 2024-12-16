# How to use Adobe campaign id 'cid' with Matomo ?

To handle custom Adobe analtics campaign parmeters `cid`, you have to tweak your Matomo Configuration.

## Step 1 : Create a new variable

In your Matomo Tag Manager, create a new variable of type "Custom JavaScript".
Copy, adapt and paste the code below :

```javascript
function (){

    // Adapt this variable to match your needs
    let matchingParams = {
        'se': 'mtm_campaign',
        'ch': 'mtm_medium',
        'sr': 'mtm_source',
        'kw': 'mtm_keyword',
    };




    // Do not change anything else below
    const queryString = window.location.search;
    let params = new URLSearchParams(queryString);
    let cid = params.get('cid');
    let finalURL = new URL(window.location.href);

    if(cid){
        // Splitting cid param to get each values
        let splittedCid = cid.split(':')

        // Attach Abode param to MTM 
        let campaignParams = {}
        splittedCid.forEach(element => {
            let campaignValue = element.split('-');
            campaignParams[matchingParams[campaignValue[0]]] = campaignValue[1];
        });

        // Append new mtm params to URL
        Object.keys(campaignParams).forEach(keyword => {
            finalURL.searchParams.append(keyword, campaignParams[keyword])
        });

        // Remove "cid" param from URL
        finalURL.searchParams.delete('cid');
    }

    return finalURL.href;
}
```

You can rename your variable "AdobePageUrl"

## Step 2 : Use variable in Matomo tag

Once your variable is created, edit your "Matomo" page view tag in order to use the new "AdobePageUrl" variable in the "Custom URL" field.

Update the tag and publish your container