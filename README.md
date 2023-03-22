# Component Model Text

Status: draft

## Goals

* Establish soundness w.r.t. Unicode and existing Web standards while maintaining consensus.
* Enable the Component Model to establish symmetry with related proposals.

## Problem statement

According to the Unicode standard (Chapter 2.7, Unicode Strings), "A Unicode string data type is simply an ordered sequence of code units. Thus a Unicode 8-bit string is an ordered sequence of 8-bit code units, a Unicode 16-bit string is an ordered sequence of 16-bit code units, and a Unicode 32-bit string is an ordered sequence of 32-bit code units." Notably, "Depending on the programming environment, a Unicode string may or may not be required to be in the corresponding Unicode encoding form."

According to the ECMAScript standard (Chapter 6.1.4, The String Type), "The String type is the set of all ordered sequences of zero or more 16-bit unsigned integer values". Given that "an ordered sequence of zero or more 16-bit unsiged integer values" as defined by the ECMAScript standard is equivalent to "an ordered sequence of 16-bit code units" as defined by the Unicode standard, it follows that an ECMAScript string is a Unicode string.

The Component Model's `string` type is currently equivalent to Unicode text that requires strings to be in the corresponding Unicode encoding form. ECMAScript strings and strings in similar programming languages like Java, C#, Dart, Kotlin, etc. are Unicode strings that are not required to be in the corresponding Unicode encoding form. Hence, not all strings in such programming languages, including ECMAScript, can be represented by the current `string` type without irreversible information loss during normal processing.

Since the definitions of Unicode strings and Component Model strings are not identical and irreversible information loss is undesirable w.r.t. supporting the composition of overall applications of components written in various programming languages, including maintaining compatibility and interoperability with ECMAScript and existing Web platform APIs, advancing the definitions of the Component Model's data types in accordance with the Unicode standard's definitions is advisable.

## Proposal

Since the Component Model's `string` type is not equivalent to Unicode strings, rename the `string` type to `text` — a non-functional change. Consequently, soundness w.r.t. Unicode is established and the Component Model is enabled to subsequently establish soundness w.r.t. existing Web standards and symmetry with related proposals.

## Recommended follow-up work

With the Component Model's `string` type name vacant, introduce a `string` type representing Unicode strings:

For the purpose of specification coherent with the Component Model's terminology, a Component Model `string` is defined as a list of Unicode code points except surrogate pairs, where surrogate pairs are substituted with their respective supplementary code point when a `string` is materialized from code units. The definition is equivalent to the definition of the separately proposed `stringref` data type, consequently establishing symmetry with the Reference-Typed Strings proposal. Notably, both types allow for the presence of isolated surrogates in normal processing. The WebIDL standards's recommendation that "Specifications should only use `USVString` \[equivalent to Unicode Text that requires strings to be in the corresponding Unicode encoding form] for APIs that perform text processing and need a string of scalar values to operate on" is honored. The Web platform design principle to "Represent strings appropriately", that is "When designing a web platform feature which operates on strings, use `DOMString` \[equivalent to a Unicode string] unless you have a specific reason not to", is honored. Thus, soundness w.r.t. existing Web standards is established. The Component Model's design goals of language neutrality and Web platform integration are enhanced.

As it is common to occasionally interpret a Unicode string as text, for example when logging to console or preparing an HTTP response, introduce a coercion from `string` to `text`: Convert sequences of code units that are not in the corresponding Unicode encoding form into the Unicode replacement character. The recommended techniques for "dealing with an isolated surrogate" "whenever such strings are specified to be in a particular Unicode encoding form" as defined by the Unicode Standard are honored, here without halting processing — the technique chosen by similar Web platform features. As Unicode text that requires strings to be in the corresponding Unicode encoding form can be represented by any Unicode string, introduce a coercion from `text` to `string`: A no-op.

## References and prior art

* [Reference-Typed Strings](https://github.com/WebAssembly/stringref/blob/main/proposals/stringref/Overview.md)
* [Component Model](https://github.com/WebAssembly/component-model), specifically MVP, Explainer, Type Definitions.
* [The Unicode Standard, Version 15.0](https://www.unicode.org/versions/Unicode15.0.0/), specifically clause 2.7, Unicode Strings and 2.5, Encoding Forms.
* [ECMAScript 2023 Language Specification](https://tc39.es/ecma262/), specifically clause 6.14, The String Type.
* [Web IDL Standard](https://webidl.spec.whatwg.org/), specifically clause 2.13.19, USVString.
* [Web Platform Design Principles](https://w3ctag.github.io/design-principles/), specifically clause 8.2, Represent strings appropriately.

## Appendix: Unicode definitions

* Code point: An integer in the range \[0, 0x10FFFF].
* Surrogate code point: A code point in the range \[0xD800, 0xDFFF].
* High surrogate: A surrogate code point in the range \[0xD800, 0xDBFF].
* Low surrogate: A surrogate code point in the range \[0xDC00, 0xDFFF].
* Surrogate pair: A high surrogate followed by a low surrogate.
* Isolated surrogate: A surrogate code point not part of a surrogate pair.
* Supplementary code point: A code point in the range \[0x10000, 0x10FFFF].
* Scalar value: A code point that is not a surrogate code point.
* Replacement character: The code point with value 0xFFFD.
* Code unit: An 8-bit, 16-bit or 32-bit unit of encoded text.
* 8-bit string: An ordered sequence of 8-bit code units.
* 16-bit string: An ordered sequence of 16-bit code units.
* 32-bit string: An ordered sequence of 32-bit code units.
* String: An 8-bit, 16-bit or 32-bit string.
* Encoding form: A well-formed combination of code units. Either UTF-8, UTF-16 or UTF-32.
