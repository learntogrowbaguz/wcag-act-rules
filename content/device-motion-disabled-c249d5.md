---
title: "Device motion based changes to the content can be disabled"
permalink: /standards-guidelines/act/rules/device-motion-disabled-c249d5/
ref: /standards-guidelines/act/rules/device-motion-disabled-c249d5/
lang: en
github:
  repository: w3c/wcag-act-rules
  path: content/device-motion-disabled-c249d5.md
# footer: > # Text in footer in HTML
#   <p> This is the text in the footer </p>
---

{% include_relative _proposed-banner.html %}

Rule Type:
:   atomic

Rule ID:
:   c249d5

Last Modified:
:   June 3, 2021

Accessibility Requirements Mapping:
:   [2.5.4 Motion Actuation (Level A)](https://www.w3.org/TR/WCAG21/#motion-actuation)
    - **Required for conformance** to WCAG 2.1 on level A and higher
    - [Outcome](#outcome) mapping:
        - Any `failed` outcomes: success criterion is not satisfied
        - All `passed` outcomes: success criterion needs further testing
        - An `inapplicable` outcome: success criterion needs further testing

Input Aspects:
:   [DOM Tree](https://www.w3.org/TR/act-rules-aspects/#input-aspects-dom)
:   [CSS Styling](https://www.w3.org/TR/act-rules-aspects/#input-aspects-css)
:   [Accessibility tree](https://www.w3.org/TR/act-rules-aspects/#input-aspects-accessibility)

## Description

This rule checks that it is possible to disable any changes to the content of the web page resulting from device motion based events.

## Applicability

This rule applies to an [HTML document][] with an associated [Window object][] that has an [event listener list][] with one or more [event listeners][event listener] for [device orientation events][device orientation] or [device motion events][device motion].

## Expectation

For each registered [device orientation event][device orientation] or [device motion event][device motion] in the test target one of the following is true:

- **no changes:** The registered event does not cause [changes to the content][changes in content] of the [web page][] within a 1 minute time span of the [event firing][event firing]; or
- **disabled:** There is at least one [set of clearly labeled instruments][] to [block the event][blocked event][] for at least 1 minute.

## Assumptions

- The motion to operate the device is not used through an [accessibility supported][] interface, which is listed as a valid exception to [Success Criterion 2.5.4: Motion Actuation][sc2.5.4].
- The motion is not [essential][] for the functionality it triggers, which is listed as a valid exception to [Success Criterion 2.5.4: Motion Actuation][sc2.5.4].
- The event listeners listening to device motion events trigger a functionality in the web page. If they do not trigger any such functionality failing this rule might not be a failure of the success criterion.
- If there are ways to disable the device motion based functionality that do not require the user to interact with the web page (e.g. a setting at the operating system level), failing this rule might not be a failure of the success criterion.
- This rule assumes that there are no changes in the content of the [web page][] caused by another [event][]. If this is not the case, changes may be attributed to the wrong [event][] and the rule may fail while [Success Criterion 2.5.4: Motion Actuation][sc2.5.4] is still satisfied.
- This rule assumes that the changes happen within a 1 minute time span after the [event][] [firing][] and therefore the comparison between the page before and after the [event][] [firing][] can be made at any time after that time span elapses. If there are changes after this time span, they may not be detected as [changes in content][] and the rule may pass but [Success Criterion 2.5.4: Motion Actuation][sc2.5.4] is not satisfied. The arbitrary 1 minute time span, selected so that testing this rule would not be impractical, is not included in WCAG.
- After being disabled, the event remains disabled until being re-enabled again. If the event is re-enabled through other non-user controlled means (e.g. a timeout) then this rule may pass while [Success Criterion 2.5.4: Motion Actuation][sc2.5.4] is not satisfied.

## Accessibility Support

_There are no major accessibility support issues known for this rule._

## Background

- [Understanding Success Criterion 2.5.4: Motion Actuation][sc2.5.4]
- [G213: Provide conventional controls and an application setting for motion activated input](https://www.w3.org/WAI/WCAG21/Techniques/general/G213.html)
- [DeviceOrientation Event Specification](https://www.w3.org/TR/orientation-event/)

## Test Cases

### Passed

#### Passed Example 1

This [HTML document][] has device orientation events that cause **no changes** to the content of the web page.

```html
<html>
	<head>
		<title>Passed Example 1</title>
		<script>
			function activateEvent() {
				let counter = 0
				window.addEventListener('deviceorientation', () => {
					counter++
				})
			}
		</script>
	</head>

	<body onload="activateEvent();">
		<p>ACT-R</p>
		<p>Note: This example may not work across all browsers.</p>
	</body>
</html>
```

#### Passed Example 2

This [HTML document][] that can be operated through the device's orientation to increase and decrease the value of a slider has a control to **disable** that functionality.

```html
<html>
	<head>
		<title>Passed Example 2</title>
		<script src="/test-assets/7677a9/slider.js"></script>
		<script>
			function activateSlider() {
				window.addEventListener('deviceorientation', handleOrientationCanBeDisabled)
			}
		</script>
	</head>

	<body onload="activateSlider();">
		<h1>Slider Motion Sensor Example</h1>

		<p>
			Open this slider on a device with a motion sensor, such as a smartphone or tablet. Tilt the device to the right
			and left to adjust the slider value. The check box disables the motion sensing adjustment.
		</p>
		<p>Note: This example may not work across all browsers.</p>

		<div>
			<input type="range" min="1" max="100" value="50" id="motionSlider" disabled />
			<p aria-live="polite">Slider Value: <span id="output">50</span></p>
		</div>
		<div>
			<input type="checkbox" id="disableMotion" />
			<label for="disableMotion">Disable Motion Actuation</label>
		</div>
	</body>
</html>
```

#### Passed Example 3

This [HTML document][] that can be operated by rotating the device to increase and decrease the value of a slider has a control to **disable** that functionality.

```html
<html>
	<head>
		<title>Passed Example 3</title>
		<script src="/test-assets/7677a9/slider.js"></script>
		<script>
			function activateSlider() {
				window.addEventListener('devicemotion', handleMotionCanBeDisabled)
			}
		</script>
	</head>

	<body onload="activateSlider();">
		<h1>Slider Motion Sensor Example</h1>

		<p>
			Open this slider on a device with a motion sensor, such as a smart phone or tablet. Rotate the device to adjust
			the slider value. The check box disables the motion sensing adjustment.
		</p>
		<p>Note: This example may not work across all browsers.</p>

		<div>
			<input type="range" min="1" max="100" value="50" id="motionSlider" disabled />
			<p aria-live="polite">Slider Value: <span id="output">50</span></p>
		</div>
		<div>
			<input type="checkbox" id="disableMotion" />
			<label for="disableMotion">Disable Motion Actuation</label>
		</div>
	</body>
</html>
```

### Failed

#### Failed Example 1

This [HTML document][] that can be operated through the device's orientation to increase and decrease the value of a slider but has no way to **disable** this functionality.

```html
<html>
	<head>
		<title>Failed Example 1</title>
		<script src="/test-assets/7677a9/slider.js"></script>
		<script>
			function activateSlider() {
				window.addEventListener('deviceorientation', handleOrientation)
			}
		</script>
	</head>

	<body onload="activateSlider();">
		<pre class="output"></pre>

		<h1>Slider Motion Sensor Example</h1>

		<p>
			Open this slider on a device with a motion sensor, such as a smartphone or tablet. Tilt the device to the right
			and left to adjust the slider value.
		</p>
		<p>Note: This example may not work across all browsers.</p>

		<div>
			<input type="range" min="1" max="100" value="50" id="motionSlider" disabled />
			<button id="increaseSlider" type="button">Increase Value</button>
			<p aria-live="polite">Slider Value: <span id="output">50</span></p>
		</div>
	</body>
</html>
```

### Inapplicable

#### Inapplicable Example 1

This [HTML document][] is not operable by device motion.

```html
<p>ACT-Rules</p>
```

## Glossary

{% include_relative glossary/blocked-event.md %}
{% include_relative glossary/changes-in-content.md %}
{% include_relative glossary/clearly-labeled-location.md %}
{% include_relative glossary/explicit-role.md %}
{% include_relative glossary/focusable.md %}
{% include_relative glossary/hidden-state.md %}
{% include_relative glossary/implicit-role.md %}
{% include_relative glossary/included-in-the-accessibility-tree.md %}
{% include_relative glossary/instrument-to-achieve-an-objective.md %}
{% include_relative glossary/marked-as-decorative.md %}
{% include_relative glossary/outcome.md %}
{% include_relative glossary/semantic-role.md %}
{% include_relative glossary/set-of-clearly-labeled-instruments.md %}
{% include_relative glossary/wai-aria-specifications.md %}
{% include_relative glossary/web-page-html.md %}

## Acknowledgements

This rule was written in the [ACT Rules community group](https://w3.org/community/act-r/), 
with the support of the EU-funded [WAI-Tools Project](https://www.w3.org/WAI/about/projects/wai-tools/).

### Authors

- [Carlos Duarte](https://github.com/carlosapaduarte)
- [João Vicente](https://github.com/joao-vicente)

## Changelog

This is the first version of this ACT rule.

[accessibility supported]: https://www.w3.org/WAI/WCAG21/Understanding/motion-actuation#dfn-accessibility-supported
[blocked event]: #blocked-event 'Definition of blocked event'
[changes in content]: #changes-in-content 'Definition of changes in content'
[device motion]: https://www.w3.org/TR/orientation-event/#devicemotion 'Definition of device motion event'
[device orientation]: https://www.w3.org/TR/orientation-event/#deviceorientation 'Definition of device orientation event'
[essential]: https://www.w3.org/WAI/WCAG21/Understanding/motion-actuation.html#dfn-essential
[event]: https://dom.spec.whatwg.org/#concept-event 'Definition of event'
[event firing]: https://dom.spec.whatwg.org/#concept-event-fire
[event listener]: https://dom.spec.whatwg.org/#concept-event-listener
[event listener list]: https://dom.spec.whatwg.org/#eventtarget-event-listener-list
[firing]: https://dom.spec.whatwg.org/#concept-event-fire 'Definition of event firing'
[html document]: https://dom.spec.whatwg.org/#concept-document
[sc2.5.4]: https://www.w3.org/WAI/WCAG21/Understanding/motion-actuation.html
[set of clearly labeled instruments]: #set-of-clearly-labeled-instruments 'Definition of set of clearly labeled instruments'
[web page]: #web-page-html 'Definition of web page'
[window object]: https://html.spec.whatwg.org/multipage/window-object.html#dom-window