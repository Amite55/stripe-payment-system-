# stripe-payment-system- with react native ------------------------------- ⬇️

## Step one
- add project RootLayout StripeProvider and aded publishabloe key

  ### Example :

      <StripeProvider
      publishableKey={
        "pk_test_51QKAtBKOpUtqOuW1x5VdNqH3vG7CZZl1P6V3VuV1qsRUmPLNk26i34AXeu2zCO3QurFJAOZ9zfb0EkWeCVhqBYgH008X41cXr6"
      }
      // merchantIdentifier="merchant.identifier" // required for Apple Pay
      // urlScheme="your-url-scheme" // required for 3D Secure and bank redirects
    
    ---------- your appp ---------------
</StripeProvider>

## step second 

- Now create payment intent 
- And push the item 

## step three
- Now add checkout then hit the payment checkout

## exmple :


// Payment login for booking start

  const [createIntent, intentResult] = useBookingIntentMutation();
  const [bookingSuccess, bookingResult] = useBookingSuccessMutation();

  const handleSetupInitialPayment = async (item: any) => {
    try {
      const res = await createIntent({
        payment_method: "pm_card_visa",
        amount: item?.price,
        service_name: item?.service_name,
      }).unwrap();
      item.stripe_payment_intent_id = res?.data?.id;

      const client_secret = res?.data?.client_secret;

      const { error } = await initPaymentSheet({
        merchantDisplayName: "Example, Inc.",
        paymentIntentClientSecret: client_secret, // retrieve this from your server
      });
      if (error) {
        // handle error
        Alert.alert("Warning", error?.message);
      } else {
        checkout(item);
      }
    } catch (error) {
      Alert.alert("Warning", "Something is wrong");
      console.log("payment", error);
    }
  };

  const checkout = async (item: any) => {
    const { error } = await presentPaymentSheet();

    if (error) {
      // handle error
      Alert.alert("Warning", error?.message);
    } else {
      // success
      try {
        const res = await bookingSuccess(item).unwrap();
        if (res?.status) {
          router.replace("/drewer/home");
        }
      } catch (error) {
        alert((error as any)?.message || "something happened wrong");
        console.log(error);
      }
    }
  };

call the handleSetupInitialPayment function -------------->
