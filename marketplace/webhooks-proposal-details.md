# Webhooks to receive information from the Marketplace

## Foreword

Please feel free to add to this file if you have valuable input.

## The situation

From my conversations with members of the core team I would say that anything we ask for has to respect 2 rules:

- Be asked for by an important enough number of people
- Not require an extraordinary amount of work for the core team as they're already stretched thin

On the other hand we need information that allow us to

- Have some visibility as to what is happening on the marketplace
- Gather some useful data that we can acto on
- Eventually get in touch with buyers (we will not get emails so no need to ask for that)

Concerning page views and that kind of data, my understanding is that it would be complicated to implement. What's more, as there is a way to do it by ourselves using pixel trackers, this is probably not the best thing to ask for.

So I was thinking about ways to get some data that would help us establish contact with buyers and eventually own our own list.

## A possible solution or first step

There is a feature of the marketplace that seems totally overlooked and which could potentially bore huge benefits: the custom help page.

When adding a package to the marketplace, we are given the choice between using the marketplace support system or providing a link to our own support page.

This is a great idea but extremely difficult to pull off without Marketplace data.

Right now, if we want to use our own support system, we have to manually check license numbers against support requests which sucks.

## Using webhooks

Ideally we would be able to set an optional webhook URL per package we have in the marketplace.

On certain events, the marketplace would send a very simple JSON object to the hook and then it's up to us to use it or not.

The events and data sent for each would be:

- On sale

````
    {
        "id": "xxx",
        "event": "sale",
        "created": 1507929225,
        "data": {
            "object": {
                "handle": "package_handle",
                "licence": "xx45s4f5fs",
                "buyer": "buyer_username",
                "buyer_id": 458756,
                "quantity": 1,
                "amount": 1500,
                "amount_perceived": 1050,
            }
        }
    }
````

- On refund

````
    {
        "id": "xxx",
        "event": "refund",
        "created": 1507929225,
        "data": {
            "object": {
                "handle": "package_handle",
                "licence": "xx45s4f5fs",
                "buyer": "buyer_username",
                "buyer_id": 458756,
                "amount_refunded": 1500,
                "amount_withdran": 1050,
            }
        }
    }
````

- On license attached to project

````
    {
        "id": "xxx",
        "event": "license_attached",
        "created": 1507929225,
        "data": {
            "object": {
                "handle": "package_handle",
                "licence": "xx45s4f5fs",
                "buyer": "buyer_username",
                "buyer_id": 458756,
                "project_name": "some name",
                "project_url": "some url",
            }
        }
    }
````

- On license removed from project

````
    {
        "id": "xxx",
        "object": "license_detached",
        "created": 1507929225,
        "data": {
            "object": {
                "handle": "package_handle",
                "licence": "xx45s4f5fs",
                "buyer": "buyer_username",
                "buyer_id": 458756,
                "project_name": "some name",
                "project_url": "some url",
            }
        }
    }
````

- On marketplace page visit

````
    {
        "object": "marketplace_page_visited",
        "created": 1507929225,
        "data": {
            "object": {
                "handle": "package_handle",
            }
        }
    }
````

For the license data we might need an object of arrays as I am not sure how it works for bulk sales.

## Benefits

### For the Core team

These are not trully benefits as much as an outline of how it wouldn't bee too much of a hassle.

This webhook system wouldn't require any added pages or front-end modification or interaction of any significance.

It would be as simple as adding a text box for us to input our webhook URL.

After that, all it would take is events (which might already be in place) and a CURL call or other to hit a hook on selected events sending it a simple set of data.

### For the sellers

This would allow us to have a real support system that we could manage ourselves with the following benefits:

- Users will have to register as they do on the marketplace and provide us with an email address which opens the possibility to keep them updated on what we do
- It will become easier to enforce the "1 month of free support then pay for support" rule. The support system would simply redirect to a payment form if the license is old
- We will find out if someone is really taking their license for a ride, attaching and detaching it from projects (Franz suggested some do that often) and we can then act accordingly

Even without a self-hosted support system, benefits from getting this data in a way that we can manage automatically would help with accounting for instance.

The webhook we provide could also be linked to a script that would send the data to Google Analytics or any other system.

I am sure others can find creative ways to use this kind of information
