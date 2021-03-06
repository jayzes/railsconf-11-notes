## Living Social - Processing Transactions at Volume
- Started slow
- 1 million on Jan 19th - $10 for $20 at Amazon
- If at all possible, avoid dealing with cards!
  - Use someone else - amazon, kickstarter, braintree, etc.
  - Added benefit of having cards on file - nice for upping conversion rate
  - Transparent redirect from gateways - skip out altogether, nice alternative
- 3 things you need
  - Bank account
  - Merchant account (approval process from 3 days to A LOT - not a normal bank account.  Functions like a line of credit)
    - Charge monthly plus a per-transaction fee
  - Payment gateway
    - For interacting with actual cards
    - Many extras - tokenization, etc.
    - Again, monthly fee plus per-transaction fee
    - Need to make sure that it actually works with your merchant account, as this is not a given
- Steps to making it work
  - Authorization
    - Gateway -> Merchant Account -> Issuing Bank (decider) and back
    - Generally takes 2-3 seconds to occur
  - Settlement
    - This is how you actually get paid
    - Batch process, happens nightly, generally takes 2-3 days for money to show up in your account
  - Use ActiveMerchant
    - From Shopify guys
    - It's awesome
    - Normalizes out gateway-specific APIs, etc.
  - Don't ever log CC info - filter_parameter_value is your friend
    - Also Nginx, etc.
  - Tokenization (card vaults)
    - i.e. Authorize.net CIM
    - Great for recurring billing and conversion
    - Obvious security benefit - tokens are linked to your merchant account
    - Sadly, no ActiveMerchant abstraction for this - can still use the library, but not as easy to switch between providers
    - Can get around the duplicate card issue by hashing card numbers (which is okay by PCI standards, explicitly including SHA-1)
      - Not really - card search space can be narrowed by ISO standards
      - Card numbers from banks start with 3,4,5, or 6.  Frequent flyers start with 1 or 2
      - First 6 digits are the BIN (Bank ID number)
      - Commonly store the first 6 digits and last 4 digits (also okay by PCI)
      - Can brute force 50 million SHA-1 hashes per second on decent hardware
      - So net-net, don't store hashes!
- Scaling card transactions
  - API call to store transaction was the bottleneck
  - External API calls in the request cycle are evil
  - Takes >1 second a lot of the time
  - Big problem at scale
  - So...we need to background it, but there are security concerns
  - Worked with braintree to develop end-to-end encryption
    - Encrypt the info asymmetrically in the browser (cool!)
    - Can never access any of the data server-side, so security is a non-issue
    - JS snippet clones the form, appends to the end of the page, and submits it.  Need to copy over checkbox and radio button state explicitly (suck).
    - Prepends encrypted values with $bt so you can figure out in the backend whether it can safely be backgrounded
    - You could do this yourself but key management is a bigger deal
  - Wrote ActionEvent to abstract queueing systems (open-sourcing soon hopefully)

- Benefits
  - Reduced time to add a card from 3 seconds to < 180ms (request time)
  - Let them handle *massive* traffic (Amazon, Fandango both at 1M sales in 24h)
  - Boost in recovery/robustness - can recover from provider failures