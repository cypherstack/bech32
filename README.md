# `Bech32`

An implementation of the [BIP-173 specification](https://github.com/bitcoin/bips/blob/master/bip-0173.mediawiki) for general and Segwit `Bech32` encoding that optionally allows for longer input data.

## Examples

```dart
  Segwit address = segwit.decode("bc1pw508d6qejxtdg4y5r3zarvary0c5xw7kw508d6qejxtdg4y5r3zarvary0c5xw7k7grplx");
  print(address.scriptPubKey);
  // => 5128751e76e8199196d454941c45d1b3a323f1433bd6751e76e8199196d454941c45d1b3a323f1433bd6
  print(address.version);
  // => 1
```

The Lightning [BOLT #11 specification](https://github.com/lightningnetwork/lightning-rfc/blob/master/11-payment-encoding.md) permits longer inputs than are allowed by BIP-173.
Use the positional `maxLength` parameter to override the default limit, which is 90 characters for the entire encoded string.
```dart
  String paymentRequest = "lnbc1pvjluezpp5qqqsyqcyq5rqwzqfqqqsyqcyq5rqwzqfqqqsyqcyq5rqwzqfqypqdpl2pkx2ctnv5sxxmmwwd5kgetjypeh2ursdae8g6twvus8g6rfwvs8qun0dfjkxaq8rkx3yf5tcsyz3d73gafnh3cax9rn449d9p5uxz9ezhhypd0elx87sjle52x86fux2ypatgddc6k63n7erqz25le42c4u4ecky03ylcqca784w";
  Bech32Codec codec = Bech32Codec();
  Bech32 bech32 = codec.decode(
    paymentRequest,
    paymentRequest.length,
  );
  print("hrp: ${bech32.hrp}");
  // => hrp: lnbc
```

Even if you set `maxLength` to a longer value than the default, the human-readable encoding prefix is still limited to 83 characters.
Modifying the length limit is not strictly compliant with BIP-173, but is fairly common in practice and consistent with other designs like the [ZIP-173 specification](https://zips.z.cash/zip-0173).

## Exceptions

The specification defines a myriad of cases in which decoding and encoding should fail.
Please make sure your code catches all the relevant exception defined in `lib/exceptions.dart`.
