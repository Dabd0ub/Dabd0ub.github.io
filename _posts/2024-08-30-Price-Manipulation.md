---
title: "Price Manipulation"
date: 2024-08-30 00:00:00 +0800
categories: [Write-Ups-Bugs]
tags: [Price Manipulation]
---

# Price Manipulation: Subscription Plans for Just 1 INR

During a pentesting engagement at [Cyber AR](https://cyberar.io/) , [Me](https://www.linkedin.com/in/dabd0ub/) and [Mohamed Allam](https://www.linkedin.com/in/m0allam) discovered a vulnerability that allows users to manipulate pricing in certain systems. Imagine subscribing to a premium service with all the perks at an unbelievable price of just 1 INR. It sounds like a great deal—except it’s not supposed to be possible. But in some cases, vulnerable systems allow users to manipulate key parameters and exploit the checkout process, making it feasible to subscribe to services at practically no cost.

In this post, We’ll break down how a pricing manipulation flaw can allow a user to adjust subscription prices on-the-fly and pay a ridiculously low amount, simply by intercepting and modifying data before it’s processed. This type of flaw highlights why price validation on both the client and server sides is essential for online platforms.

---

### Subscription Price Manipulation

Let’s walk through how this vulnerability plays out on a typical platform with subscription-based services. We’ll use the example of a website offering annual and monthly plans, with multiple subscription tiers.

The key to this exploit lies in intercepting the requests between the browser and the server during the subscription checkout process. By changing the price-related parameters in these requests, a user can drastically reduce the price they are charged for the subscription—often to absurdly low amounts like 1 INR.

---

### Step-by-Step Breakdown of the Exploit

1. **Selecting a Subscription Plan**:
The first step is typical for any user. You navigate to the subscription page, like https://target.com/#/add-subscription, select either the annual or monthly plan, and click on “Add This Subscription.”
2. **Proceed to Checkout**:
Once you’ve picked your plan, the next step is to click “Subscribe Now.” This is where the request for payment processing is initiated, and the system calculates the total price, subtotal, and other cost-related parameters.
3. **Intercept the Request**:
Before clicking "Continue to Payment," intercept the request using a tool like Burp Suite or any other proxy tool. At this stage, the request will contain the subscription price, subtotal, and grand total that are being sent to the server for processing.
4. **Modify the Price Parameters**:
Here’s where the magic happens. In the intercepted request, you’ll find parameters such as `price`, `sub_total`, and `grand_total`. By manually changing these values to something like `1.0000`, you can effectively set the total price of the subscription to 1 INR.
5. **Continue the Payment Process**:
After manipulating the values, forward the request and allow the process to continue. The platform will likely redirect you to the payment gateway or merchant’s page to complete the transaction, but this time with the manipulated, lower price.
6. **Enjoy Your Subscription**:
Assuming the system doesn’t have proper validation in place, the payment will go through with the drastically reduced price, allowing you to enjoy a premium service for a fraction of what it should actually cost.

---

### Impact

The implications of this vulnerability are significant, especially for platforms that rely on subscription revenue. Not only does this lead to a direct financial loss, but it can also open the door to more sophisticated attacks or fraud schemes. Moreover, users exploiting such vulnerabilities can access services at discounted rates or even for free, disrupting the business model of the platform.

For example, a company offering subscription services for premium content, software, or even e-commerce platforms could lose considerable revenue if this flaw is exploited at scale.

---

### Preventing Price Manipulation

To protect against price manipulation, platforms need to implement robust validation mechanisms that ensure the integrity of all payment-related data. Here’s how this can be done:

1. **Server-Side Validation**: The server should always perform its own calculations for the price, subtotal, and grand total, rather than relying on values sent by the client. Any discrepancies between the server-calculated values and the client-provided values should trigger an alert or block the transaction.
2. **Price Lookup on the Server**: Instead of allowing the client to send the price directly, the server should fetch the price associated with the product or subscription plan based on its own database. This ensures that the price sent to the payment gateway is accurate and tamper-proof.
3. **Digital Signatures or Hashing**: Implementing signed or hashed data for critical parameters can help detect tampering. If any price-related data is modified during transmission, the signature or hash will no longer match, flagging the transaction as potentially malicious.
4. **Monitoring Suspicious Transactions**: Regularly monitoring transactions that involve unusually low prices or frequent payment failures can help identify users attempting to exploit such vulnerabilities.

---

### Final Thoughts

Price manipulation vulnerabilities might seem simple, but their impact can be devastating to businesses offering online services. It’s a reminder that security doesn’t stop at protecting sensitive data—it also extends to protecting business processes like pricing and payments. By securing the communication between client and server and enforcing strict validation, platforms can avoid losing revenue to these types of exploits and maintain the trust of their users.
