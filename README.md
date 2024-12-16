# adobe-campaign-formatter-for-matomo-tag-manager

```javascript
function (){
            const queryString = window.location.search;
            let params = new URLSearchParams(queryString);
            let cid = params.get('cid');
            let finalURL = new URL(window.location.href);

            if(cid){
                // Splitting cid param to get each values
                let splittedCid = cid.split(':')

                // Match Adobe to mtm or utm params
                let matchingParams = {
                    'se': 'mtm_campaign',
                    'ch': 'mtm_medium',
                    'sr': 'mtm_source',
                    'kw': 'mtm_keyword',
                };

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