---
title: Web Accessibility 3 - Semantics Basics
created: '2020-09-09T07:09:04.708Z'
modified: '2020-09-12T11:00:29.876Z'
---

# Web Accessibility 3 - Semantics Basics

## Assistive Technology

* An umbrella term for a broad range of devices, software and tools that helps someone complete their task.

## Affordances

* Affordances represent actions we can take that we have learnt with visual clues.
  * Buttons and sliders are classic examples of what we are used to.

## Semantics and Assistive Technology

* We need to make sure that information is expressed in a way that allows it to be used by assistive technology.
* Web AIM WCAG 4.1.2: Name, Role, Value: markup is used in a way that facilitates accessibility.
* Role - What type of element it is?
  * Might name, might be a sound.
* Name/Value - if it has one.
* Any information about that elements state.

## The Accessibility Tree

* The browser takes the DOM tree and turns it into a form which is useful for assistive technology:
* It can be helpful to think of an accessibility tree as a webpage from the 90s, basic formatting but the info is there.

```html

<main>
  <!-- <div class="card"> -->
    <form>
      <!-- <div class="trip-selector"> -->
        <!-- <div class="row"> -->
          <!-- <div class="inline-control col-2"> -->
            <label for="seatType"> Seat type</label>
            <select name="seatType" id="seatType">
              <option value="0">No preference</option>
              <option value="1">Aisle seat</option>
              <option value="2">Window seat</option>
            </select>
          <!-- </div> -->
          <!-- <div class="inline-control submit-form col-1"> -->
            <button type="submit" id="submitBtn">Search</button>
          <!-- </div> -->
        <!-- </div> -->
      <!-- </div> -->
    </form>
  <!-- </div> -->
</main>

```

## Writing Semantic HTML

* Provide text alternatives for any non-text content
* We generally use label for name (although there are other alternatives)
  * Visible labels and text alternatives.
    * Alternatives like the alt tag in images.
* Recommendations:
  * All images, form image buttons, and image map hot spots have appropriate, equivalent alternative text.
  * Images that do not convey content, are decorative, or contain content that is already conveyed in text are given null alt text (alt="") or implemented as CSS backgrounds.
    * All linked images have descriptive alternative text.
  * Form buttons have a descriptive value.
  * Form inputs have associated text labels.
    * Rather than just putting some text next to a button, we need to use the label element.

### Text Alternatives

* Alt is only used if the image isn't available.
* If there is no alt text tag, the filename will be read out. Therefore you need to use `alt=""` if you don't want alt text.



