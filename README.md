# Chase Offer Claimer

Do you have a Chase credit card?  
Do you wish you could automatically apply all of the offers?  
Well now you can!

1. Log into your Chase account using a desktop web browser.
2. Go to the Chase Offers page and select an account.
3. Paste this into your browser's developer console:

```javascript
(function claimAllOffers() {
    var notAddedFilter = document.querySelector('mds-checkbox[label="Not added"]');
    var seeAllBtn = document.querySelector('mds-button[text="See all offers"]');
    var unclaimedOffers = document.querySelectorAll('div[data-cy="commerce-tile"]:has(mds-icon[type="ico_add_circle"])');
    var noOffersMsg = document.querySelector('div[data-testid="no-offers-container"]');

    if (document.location.hash.indexOf('offer-hub') !== -1 && seeAllBtn != null) {
        // This is the offers landing page but we need the main offer listing.
        seeAllBtn.click();
    } else if (notAddedFilter != null && !notAddedFilter.checked) {
        // If not enabled, click the "Not added" filter.
        notAddedFilter.click();
    } else if (document.location.hash.indexOf('offer-activated') !== -1) {
        // We have activated an offer, go back to the main offer listing.
        history.back();
    } else if (unclaimedOffers.length) {
        // If there is an unclaimed offer, claim it.
        var firstOffer = unclaimedOffers[0];
        var offerText = firstOffer.innerText.replaceAll('\n', ' - ');
        console.log('Claiming offer: ' + offerText);
        firstOffer.click();
    } else if (noOffersMsg != null) {
        // If the "We couldn't find a match" message is present then all offers have been claimed.
        console.log('All done!');
        return;
    }
    setTimeout(claimAllOffers, 250);
})();
```

... and that's it!  
The script takes a minute or two to run depending on the number of available offers.  Typically it can add around 50 offers a minute but your performance may vary.

**Pro tip:** convert the snippet into a [bookmarklet](https://caiorss.github.io/bookmarklet-maker/) for easier reuse!
