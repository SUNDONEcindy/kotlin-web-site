[//]: # (title: Kotlin language features and proposals)

<web-summary>Learn about the lifecycle of Kotlin features. The page contains the full list of Kotlin language features and design proposals.</web-summary>

JetBrains evolves the Kotlin language according to the [Kotlin language evolution principles](kotlin-evolution-principles.md),
guided by pragmatic design.

> Language feature proposals are listed from Kotlin 1.7.0. 
> 
> See the explanation of language feature statuses in the 
> [Kotlin evolution principles documentation](kotlin-evolution-principles.md#pre-stable-features).
> 
{style="note"}

<tabs>
<tab id="all-proposals" title="All">

<!-- <include element-id="all-proposals" from="all-proposals.topic"/> -->

<snippet id="source">
<table style="header-column">

<!-- EXPLORATION AND DESIGN BLOCK -->

<tr filter="exploration-and-design">
<td width="200">

**Exploration and design**

</td>
<td>

**Rich Errors: Error union types**

* KEEP proposal: Not defined
* YouTrack issue: [KT-68296](https://youtrack.jetbrains.com/issue/KT-68296)

</td>
</tr>

<tr filter="exploration-and-design">
<td>

**Exploration and design**

</td>
<td>

**Name-based destructuring**

* KEEP proposal: [name-based-destructuring.md](https://github.com/Kotlin/KEEP/blob/name-based-destructuring/proposals/name-based-destructuring.md)
* YouTrack issue: [KT-19627](https://youtrack.jetbrains.com/issue/KT-19627)

</td>
</tr>

<tr filter="exploration-and-design">
<td>

**Exploration and design**

</td>
<td>

**Support immutability**

* KEEP notes: [immutability](https://github.com/Kotlin/KEEP/blob/master/notes/value-classes.md#immutability-and-value-classes)
* YouTrack issue: [KT-77734](https://youtrack.jetbrains.com/issue/KT-77734)

</td>
</tr>

<!-- END OF EXPLORATION AND DESIGN BLOCK -->

<!-- KEEP DISCUSSION BLOCK -->

<tr filter="keep">
<td width="200">

**KEEP discussion**

</td>
<td>

**Kotlin statics and static extensions**

* KEEP proposal: [statics.md](https://github.com/Kotlin/KEEP/blob/static-scope/proposals/static-member-type-extension.md)
* YouTrack issue: [KT-11968](https://youtrack.jetbrains.com/issue/KT-11968)

</td>
</tr>

<tr filter="keep">
<td>

**KEEP discussion**

</td>
<td>

**Collection literals**

* KEEP proposal: [collection-literals.md](https://github.com/Kotlin/KEEP/blob/bobko/collection-literals/proposals/collection-literals.md)
* YouTrack issue: [KT-43871](https://youtrack.jetbrains.com/issue/KT-43871)

</td>
</tr>

<tr filter="keep">
<td>

**KEEP discussion**

</td>
<td>

**Explicit backing fields**

* KEEP
  proposal: [explicit-backing-fields.md](https://github.com/Kotlin/KEEP/blob/explicit-backing-fields/proposals/explicit-backing-fields.md)
* YouTrack issue: [KT-14663](https://youtrack.jetbrains.com/issue/KT-14663)

</td>
</tr>

<tr filter="keep">
<td>

**KEEP discussion**

</td>
<td>

**Version overloading**

* KEEP
  proposal: [version-overloading.md](https://github.com/Kotlin/KEEP/blob/version-overloading-proposal/proposals/version-overloading.md)

</td>
</tr>

<tr filter="keep">
<td>

**KEEP discussion**

</td>
<td>

**Unused return value checker**

* KEEP
  proposal: [unused-return-value-checker.md](https://github.com/Kotlin/KEEP/blob/underscore-for-unused-local/proposals/unused-return-value-checker.md)
* YouTrack issue: [KT-12719](https://youtrack.jetbrains.com/issue/KT-12719)

</td>
</tr>

<tr filter="keep">
<td>

**KEEP discussion**

</td>
<td>

**Streamline KDoc ambiguity links**

* KEEP proposal: [streamline-KDoc-ambiguity-references.md](https://github.com/Kotlin/KEEP/blob/kdoc/Streamline-KDoc-ambiguity-references/proposals/kdoc/streamline-KDoc-ambiguity-references.md)
* GitHub issues: [dokka/#3451](https://github.com/Kotlin/dokka/issues/3451), [dokka/#3179](https://github.com/Kotlin/dokka/issues/3179), [dokka/#3334](https://github.com/Kotlin/dokka/issues/3334)

</td>
</tr>

<tr filter="keep">
<td>

**KEEP discussion**

</td>
<td>

**Resolution of links to extensions in KDoc**

* KEEP proposal: [links-to-extensions.md](https://github.com/Kotlin/KEEP/blob/kdoc/extension-links/proposals/kdoc/links-to-extensions.md)
* GitHub issue: [dokka/#3555](https://github.com/Kotlin/dokka/issues/3555)

</td>
</tr>

<!-- END OF KEEP DISCUSSION BLOCK -->

<!-- IN PREVIEW BLOCK -->

<tr filter="in-preview">
<td width="200">

**In preview**

</td>
<td>

**Context parameters: support for context-dependent declarations**

* KEEP
  proposal: [context-parameters.md](https://github.com/Kotlin/KEEP/blob/context-parameters/proposals/context-parameters.md)
* YouTrack issue: [KT-14663](https://youtrack.jetbrains.com/issue/KT-10468)
* Available since: 2.2.0

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**Improvements to annotation use-site targets on properties**

* KEEP
  proposal: [Improvements to annotation use-site targets on properties](https://github.com/Kotlin/KEEP/blob/change-defaulting-rule/proposals/change-defaulting-rule.md)
* YouTrack issue: [KT-19289](https://youtrack.jetbrains.com/issue/KT-19289)
* Available since: 2.2.0

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**Nested (non-capturing) type aliases**

* KEEP
  proposal: [Nested (non-capturing) type aliases](https://github.com/Kotlin/KEEP/blob/nested-typealias/proposals/nested-typealias.md)
* YouTrack issue: [KT-45285](https://youtrack.jetbrains.com/issue/KT-45285)
* Available since: 2.2.0

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**Context-sensitive resolution**

* KEEP
  proposal: [context-sensitive-resolution.md](https://github.com/Kotlin/KEEP/blob/improved-resolution-expected-type/proposals/context-sensitive-resolution.md)
* YouTrack issue: [KT-16768](https://youtrack.jetbrains.com/issue/KT-16768)
* Available since: 2.2.0

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**Expose boxed inline value classes in JVM**

* KEEP
  proposal: [jvm-expose-boxed.md](https://github.com/Kotlin/KEEP/blob/jvm-expose-boxed/proposals/jvm-expose-boxed.md)
* YouTrack issue: [KT-28135](https://youtrack.jetbrains.com/issue/KT-28135)
* Available since: 2.2.0

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**kotlin.time.Instant**

* KEEP proposal: [Instant and Clock](https://github.com/Kotlin/KEEP/blob/master/proposals/stdlib/instant.md)
* Available since: 2.1.0

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**Uuid**

* KEEP proposal: [uuid.md](https://github.com/Kotlin/KEEP/blob/uuid/proposals/stdlib/uuid.md)
* YouTrack issue: [KT-31880](https://youtrack.jetbrains.com/issue/KT-31880)
* Available since: 2.0.20

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**Common Atomics and Atomic Arrays**

* KEEP
  proposal: [Common atomics](https://github.com/Kotlin/KEEP/blob/master/proposals/stdlib/common-atomics.md)
* YouTrack issue: [KT-62423](https://youtrack.jetbrains.com/issue/KT-62423)
* Available since: 2.2.0

</td>
</tr>

<tr filter="in-preview">
<td>

**In preview**

</td>
<td>

**KMP Kotlin-to-Java direct actualization**

* KEEP
  proposal: [kmp-kotlin-to-java-direct-actualization.md](https://github.com/Kotlin/KEEP/blob/kotlin-to-java-direct-actualization/proposals/kmp-kotlin-to-java-direct-actualization.md)
* YouTrack issue: [KT-67202](https://youtrack.jetbrains.com/issue/KT-67202)
* Available since: 2.1.0

</td>
</tr>

<!-- the first td element should have the width="200" attribute -->

<!-- END OF IN PREVIEW BLOCK -->

<!-- STABLE BLOCK -->

<tr filter="stable">
<td width="200">

**Stable**

</td>
<td>

**Guard conditions in when-with-subject**

* KEEP proposal: [guards.md](https://github.com/Kotlin/KEEP/blob/guards/proposals/guards.md)
* YouTrack issue: [KT-13626](https://youtrack.jetbrains.com/issue/KT-13626)
* Available since: 2.2.0

</td>
</tr>

<tr filter="stable">
<td>

**Stable**

</td>
<td>

**Multidollar interpolation: improved handling of `$` in string literals**

* KEEP proposal: [dollar-escape.md](https://github.com/Kotlin/KEEP/blob/master/proposals/dollar-escape.md)
* YouTrack issue: [KT-2425](https://youtrack.jetbrains.com/issue/KT-2425)
* Available since: 2.2.0

</td>
</tr>

<tr filter="stable">
<td>

**Stable**

</td>
<td>

**Non-local `break` and `continue`**

* KEEP
  proposal: [break-continue-in-inline-lambdas.md](https://github.com/Kotlin/KEEP/blob/master/proposals/break-continue-in-inline-lambdas.md)
* YouTrack issue: [KT-1436](https://youtrack.jetbrains.com/issue/KT-1436)
* Available since: 2.2.0

</td>
</tr>

<tr filter="stable">
<td>

**Stable**

</td>
<td>

**Stabilized `@SubclassOptInRequired`**

* KEEP
  proposal: [subclass-opt-in-required.md](https://github.com/Kotlin/KEEP/blob/master/proposals/subclass-opt-in-required.md)
* YouTrack issue: [KT-54617](https://youtrack.jetbrains.com/issue/KT-54617)
* Available since: 2.1.0

</td>
</tr>

<tr filter="stable">
<td width="200">

**Stable**

</td>
<td>

**`Enum.entries`: performant replacement of the `Enum.values()`**

* KEEP proposal: [enum-entries.md](https://github.com/Kotlin/KEEP/blob/master/proposals/enum-entries.md)
* YouTrack issue: [KT-48872](https://youtrack.jetbrains.com/issue/KT-48872)
* Target version: 2.0.0

</td>
</tr>

<tr filter="stable">
<td>

**Stable**

</td>
<td>

**Data objects**

* KEEP proposal: [data-objects.md](https://github.com/Kotlin/KEEP/blob/master/proposals/data-objects.md)
* YouTrack issue: [KT-4107](https://youtrack.jetbrains.com/issue/KT-4107)
* Target version: 1.9.0

</td>
</tr>

<tr filter="stable">
<td>

**Stable**

</td>
<td>

**RangeUntil operator `..<`**

* KEEP proposal: [open-ended-ranges.md](https://github.com/kotlin/KEEP/blob/open-ended-ranges/proposals/open-ended-ranges.md)
* YouTrack issue: [KT-15613](https://youtrack.jetbrains.com/issue/KT-15613)
* Target version: 1.7.20

</td>
</tr>

<tr filter="stable">
<td>

**Stable**

</td>
<td>

**Definitely non-nullable types**

* KEEP proposal: [definitely-non-nullable-types.md](https://github.com/Kotlin/KEEP/blob/master/proposals/definitely-non-nullable-types.md)
* YouTrack issue: [KT-26245](https://youtrack.jetbrains.com/issue/KT-26245)
* Target version: 1.7.0

</td>
</tr>

<!-- END OF STABLE BLOCK -->

<!-- REVOKED BLOCK -->

<tr filter="revoked">
<td width="200">

**Revoked**

</td>
<td>

**Context receivers**

* KEEP proposal: [context-receivers.md](https://github.com/Kotlin/KEEP/blob/master/proposals/context-receivers.md)
* YouTrack issue: [KT-10468](https://youtrack.jetbrains.com/issue/KT-10468)
* Replaced with [context-parameters.md](https://github.com/Kotlin/KEEP/blob/context-parameters/proposals/context-parameters.md)

</td>
</tr>

<tr filter="revoked">
<td>

**Revoked**

</td>
<td>

**Java synthetic property references**

* KEEP
  proposal: [references-to-java-synthetic-properties.md](https://github.com/Kotlin/KEEP/blob/master/proposals/references-to-java-synthetic-properties.md)
* YouTrack issue: [KT-8575](https://youtrack.jetbrains.com/issue/KT-8575)

</td>
</tr>

</table>
</snippet>

<!-- END OF REVOKED BLOCK -->

</tab>

<tab id="exploration-and-design" title="Exploration and design">

<include element-id="source" use-filter="empty,exploration-and-design" from="kotlin-language-features-and-proposals.md"/>

</tab>

<tab id="keep-preparation" title="KEEP discussion">

<include element-id="source" use-filter="empty,keep" from="kotlin-language-features-and-proposals.md"/>

</tab>

<tab id="in-preview" title="In preview">

<include element-id="source" use-filter="empty,in-preview" from="kotlin-language-features-and-proposals.md"/>

</tab>

<tab id="stable" title="Stable">

<include element-id="source" use-filter="empty,stable" from="kotlin-language-features-and-proposals.md"/>

</tab>

<tab id="revoked" title="Revoked">

<include element-id="source" use-filter="empty,revoked" from="kotlin-language-features-and-proposals.md"/>

</tab>
</tabs>
