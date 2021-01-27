THIS LESSON IS ABOUT ANDROID PROPERTY ANIMATIONS.

________________________________________________________________________________

                                SECOND COMMIT

________________________________________________________________________________

________________________________________________________________________________

                                    VIDEO 3

________________________________________________________________________________
Getting Set up
Get the code
In this step, you download the code for the entire lesson and then run a simple
 example app.

$ git clone https://github.com/udacity/android-kotlin-animation-property-animation
Alternatively, you
can download the repository as a Zip
file: https://github.com/udacity/android-kotlin-animation-property-animation

Unzip the code

Open the project Android Studio version 3.5 or newer.
Run the code
Build and run the application, which looks like this:


Familiarize yourself with the UI code
Because this lab focuses on animation techniques, you are not going to build the
UI that the application uses. But you should know what’s been built for you.

Step 1: Explore the UI layout file

    In your Android Studio project, navigate to activity_main.xml. Find the
    top-level container which is a ConstraintLayout. Inside that container, notice
    six buttons; you will connect these buttons to click listeners to
    launch animations.

    Find the FrameLayout, a ViewGroup container which contains a single ImageView.
    You can think of this FrameLayout as the blank background
    (the night sky, if you will) that you will paint your animations onto, using
    ImageViews. The ImageView exists to hold the star graphic used to
    demonstrate most of the animations in this lesson.

    Now click on each of the buttons: notice how every one of them does…
    absolutely nothing.

Step 2: Get to know the activity code

    Switch to MainActivity.kt in the editor. You can see that some of the code has
    been written for you already. Specifically, we’ve created lateinit
    variables to hold the views that we will refer to in the code:

          lateinit var star: ImageView
          lateinit var rotateButton: Button
          lateinit var translateButton: Button
          lateinit var scaleButton: Button
          lateinit var fadeButton: Button
          lateinit var colorizeButton: Button
          lateinit var showerButton: Button

    You can also see that these variables are initialized to appropriate
    values in onCreate():

          star = findViewById(R.id.star)
          rotateButton = findViewById<Button>(R.id.rotateButton)
          translateButton = findViewById<Button>(R.id.translateButton)
          scaleButton = findViewById<Button>(R.id.scaleButton)
          fadeButton = findViewById<Button>(R.id.fadeButton)
          colorizeButton = findViewById<Button>(R.id.colorizeButton)
          showerButton = findViewById<Button>(R.id.showerButton)

    Next, you’ll see five methods that will be called by listeners to perform the
    functionality for the various buttons (rotater(), translater(), etc.). And
    you’ll see that those functions are empty; this is where you will write your
    code in the following steps.

    Finally, you can see the rest of the code in onCreate(), in which we set up
    onClick listeners for each of the buttons, calling into the (currently) empty
    functions.

          rotate.setOnClickListener {
              rotater()
          }

          translate.setOnClickListener {
              translater()
          }

          scale.setOnClickListener {
              scaler()
          }

          fade.setOnClickListener {
              fader()
          }

          bgColor.setOnClickListener {
              colorizer()
          }

          shower.setOnClickListener {
              shower()
          }
________________________________________________________________________________

                              VIDEO 4
________________________________________________________________________________

Rotate The Star

In this step, you will implement the rotater() function for the rotateButton
click handler, which will cause the star to rotate in a circle.

Step 1: Creating the animator

  //TODO 1.1
  1. Inside therotater() function, create an animation that rotates the ImageView
     containing the star from a value of -360 to 0. This means that the view, and
     thus the star inside it, will rotate in a full circle (360 degrees) around
     its center.

            val animator = ObjectAnimator.ofFloat(star, View.ROTATION, -360f, 0f)

    Note: The reason that the animation starts at -360 is that that allows the star
    to complete a full circle (360 degrees) and end at 0, which is the default
    rotation value for a non-rotated view, so it’s a good value to have at the end
    of the animation (in case any other action occurs on that view later, expecting
    the default value).

This line of code creates an ObjectAnimator that acts on the target “star”
(the ImageView instance that holds our star graphic). It runs an animation on
the ROTATION property of star. Changes to that property will cause the star to
rotate around its center. There are two other rotation properties
(ROTATION_X and ROTATION_Y) that rotate around the other axes (in 3D), but
these are not typically used in UI animations, since UIs are typically 2D.

    Note: A property, to the animation system, is a field that is exposed via
    setters and getters, either implicitly (as properties are in Kotlin) or
    explicitly (via the setter/getter pattern in the Java programming language).
    There are also a special case of properties exposed via the class
    android.util.Property which is used by the View class, which allows a type-safe
    approach for animations, as we’ll see later. The Animator system in Android was
    specifically written to animate properties, meaning that it can animate
    anything (not just UI elements) that has a setter (and, in some cases, a getter)

    Note: View properties are a set of functionality added to the base View class
    that allows all views to be transformed in specific ways that are useful in UI
    animations. There are properties for position (called “translation”), rotation,
    scale, and transparency (called “alpha”).

There are actually two different ways to access the properties, by regular
setter/getter pairs, like setTranslateX()/getTranslateX(), and by static
android.util.Property objects, like View.TRANSLATE_X (an object that has both
a get() and a set() method). This lesson primarily uses
android.util.Propertyobjects, because it has less overhead internally along with
better type-safety, but you can also use the setters and getters of any object,
as you will see toward the end of the lab.

    Note: Property animation is, simply, the changing of property values over time.
    ObjectAnimator was created to provide a simple set-and-forget mechanism for
    animating properties. For example, you can define an animator that changes the
    “alpha” property of a View, which will alter the transparency of that view on
    the screen.

The animation runs from a start value of -360 degrees to an end value of
0 degrees, which will spin the star in a single rotation about its center.
Note that the start starts at 0 degrees, before the animation begins, and then
jumps immediately to -360 degrees. But since -360 is visually the same as
0 degrees, there is no noticeable change when the animation begins.

  2. Now you can run the app again. Click on the ROTATE button and you will
     notice… nothing. We’ve set up the animation, but we haven’t yet told it
     to run. Let’s do that.




Step 2: Running the animation

  //TODO 1.2
  1. Below the animator, add a single line that starts it:

          animator.start()

    Now if you run the application again and click on ROTATE, you will see
    that the star does, indeed, spin around its center. But it does so really
    quickly. In fact, it does it in 300 milliseconds, which is the default
    duration of all animations on the platform. 300 milliseconds is a decent
    default for most animations, but in this case, we’d like more time to enjoy
    the animation.

  //TODO 1.3
  2. Change the duration property of the animator to 1000 milliseconds by
     adding a single line of code between the previous two lines:

          val animator = ObjectAnimator.ofFloat(star, View.ROTATION, -360f, 0f)
          animator.duration = 1000
          animator.start()

    You’re almost there. If you run the app, you’ll see that it has a nice
    animation that lasts for a second. All good, right?

Step 3: Avoiding discontinuous motion

    Well… maybe you’re impatient, like I am, and you run the animation again
    before it’s come to a stop. Do you notice a jump when you click on the
    button? This is because we always reset to -360 degrees at the start of
    the animation, regardless of whether the star is currently in the middle of
    animating or not. This discontinuous motion is an example of what we call
    “jank”; it causes a disruptive flow for the user, instead of the smooth
    experience you would like.

    There are different ways of dealing with this situation (such as starting
    the new animation from whatever the current value is). But to keep things
    simple, you’re going to just prevent the user from clicking the button
    while the animation is running, to allow them to fully enjoy the in-process
    animation first.

    Animators have a concept of listeners, which call back into user code to
    notify the application of changes in the state of the animation. There are
    callbacks for an animation starting, ending, pausing, resuming, and
    repeating. What we care about here are just the start and end events; we’d
    like to disable the ROTATE button as soon as the animation starts, and then
    re-enable it when the animation ends.

    //TODO 1.4
      1. Add a new AnimatorListenerAdapter object to the animator and override
         the onAnimationStart() and onAnimationEnd() methods.

    Note: This code uses an adapter class (which provides default
    implementations of all of the listener methods) instead of implementing the
    raw AnimatorListener interface. The code only needs to know about a couple
    of the callbacks; it’s not worth implementing the rest of the listener
    methods, so we let the adapter stub them out for us.

          val animator = ObjectAnimator.ofFloat(star, View.ROTATION, -360f, 0f)
          animator.duration = 1000
          animator.addListener(object : AnimatorListenerAdapter() {
              override fun onAnimationStart(animation: Animator?) {
                  rotateButton.isEnabled = false
              }
              override fun onAnimationEnd(animation: Animator?) {
                  rotateButton.isEnabled = true
              }
          })
          animator.start()


    Here, rotateButton is disabled as soon as the animation starts and
    re-enabled when the animation ends. This way, each animation is completely
    separate from any other rotation animation, avoiding the jank of restarting
    in the middle.

    That’s it for this first task; you now have an application that can launch
    rotation animations on the star. Take it for a spin!
________________________________________________________________________________

                              VIDEO 5
________________________________________________________________________________

Translate the Star

In this task, you will wire up translateButton to an onClick listener,
which will cause the star to move back and forth. translateButton calls
the function translater(), which is currently empty. Let’s fill that in.



Step 1: Creating the animator


  //TODO 1.5
  1. Inside the translater() function, create an animation that moves the star
     to the right by 200 pixels and runs it:

          val animator = ObjectAnimator.ofFloat(star, View.TRANSLATION_X, 200f)
          animator.start()

  2. Run the application now. When you click on TRANSLATE, the star moves to
     the right… but it doesn’t come back to the center. If you click on the
     button again, it doesn’t move at all.

What’s going on?

First of all, the animation is only being set up to run one way; it animates
the star 200 pixels to the right… and that’s it. So if we want it to come
back, we’re going to need something extra.

Also, subsequent animations don’t appear to do anything because the animation
is set up to run to a value of 200. After the animation has run, the value
is already at 200, so there’s no place else to go.

We can fix both of these problems by using the concept of “repetition.”

Note: Repetition is a way of telling animations to do the same task again and
      again. You can specify how many times to repeat (or just tell it to run
      infinitely). You can also specify the repetition behavior, either REVERSE
      (for reversing the direction every time it repeats) or RESTART
      (for animating from the original start value to the original end value,
      thus repeating in the same direction every time).

  //TODO 1.6
  3. You will change the animation to repeat, playing in reverse back to its
     starting position. Set the repeatCount property on the animation
     (which controls how many times it repeats after the first run) as well
     as the type of repetition (REVERSE or RESTART for repeating again from/to
     the same values).

          val animator = ObjectAnimator.ofFloat(star, View.TRANSLATION_X, 200f)
          animator.repeatCount = 1
          animator.repeatMode = ObjectAnimator.REVERSE
          animator.start()

  4. Run the app again and you can see that the button now animates to the right
     and back. That is, unless you click the button again while it’s running,
     which causes a problem.

The problem here is different than what we saw with the rotation animation. In
that task, the star would snap back to its starting value to begin the
animation anew. But here, the animation doesn’t snap at all. Instead,
it starts animating from where it’s at, but it doesn’t go as far. For
example, if you restart it halfway through its return trip (when it
is at 100), then it will start the new animation from 100… but it
will still only go to 200. So the overall animation is much shorter
because it started from a value greater than the original starting point of 0.

What’s going on?

There’s a subtle difference between this animator and the animator used for the
rotation task. The rotation animation was given both start and end values, so it
always ran the animation between those two values. Here, the animation is
given only an end value. When the animation starts, it first queries the
current value of the translation property on star and uses that as its implicit
start value, animating from that value to 200. So when you click on the
TRANSLATE button when the animation is part of the way through, it grabs
that mid-way value as the starting value for the new animation and runs the
animation over a smaller distance from there to 200.



Step 2: Prevent restarts while the animation is running



You will fix this in a similar way to how you fixed it for the rotation
animation, by disabling the translateButton during the animation so that
the animation comes to a rest back at 0 before it can run again.

Since this is the second time you are writing very similar code
(adding a listener to enable/disable a button), you should refactor
that code into a separate function that you’ll use everywhere you need it.

  //TODO 1.7
  1. Create a function called disableViewDuringAnimation(), which takes a View
     and an Animator, and use the code you already wrote earlier in rotater()
     to create the    body of this function:

          private fun disableViewDuringAnimation(view: View, animator: ObjectAnimator) {
              animator.addListener(object : AnimatorListenerAdapter() {
                  override fun onAnimationStart(animation: Animator?) {
                      view.isEnabled = false
                  }

                  override fun onAnimationEnd(animation: Animator?) {
                      view.isEnabled = true
                  }
              })
          }

  //TODO 1.8
  2. Now call this method in translater() and rotater() to disable their
     buttons during their respective animations. Also remove the code that
     sets the click listener in the rotater() function

          private fun rotater() {
              val animator = ObjectAnimator.ofFloat(star, View.ROTATION,
                                                    -360f, 0f)
              animator.duration = 1000
              disableViewDuringAnimation(rotateButton, animator)
              animator.start()
          }

          private fun translater() {
              val animator = ObjectAnimator.ofFloat(star, View.TRANSLATION_X,
                                                    200f)
              animator.repeatCount = 1
              animator.repeatMode = ObjectAnimator.REVERSE
              disableViewDuringAnimation(translateButton, animator)
              animator.start()
          }

  3. Run the app again. You should now see that the rotation works as before,
     and that translation also enables/disables its button due to the
     View-disabling functionality you’ve added to its animator listener.



Step 3: Refactor into an extension function



As a bonus step, take advantage of Kotlin’s language features by using
extension functions.

  //TODO 1.9
  1. Change the disableViewDuringAnimation() function to be an extension function
     on ObjectAnimator. This makes the function more concise to call, since it
     eliminates a parameter. It also makes the code a little more natural to
     read, by putting the animator-related functionality directly
     onto ObjectAnimator:

          private fun ObjectAnimator.disableViewDuringAnimation(view: View) {
              addListener(object : AnimatorListenerAdapter() {
                  override fun onAnimationStart(animation: Animator?) {
                      view.isEnabled = false
                  }

                  override fun onAnimationEnd(animation: Animator?) {
                      view.isEnabled = true
                  }
              })
          }

  //TODO 1.10
  2. Modify the code in translater() to call this extension function:

          private fun translater() {
              val animator = ObjectAnimator.ofFloat(star, View.TRANSLATION_X,
                                                    200f)
              animator.repeatCount = 1
              animator.repeatMode = ObjectAnimator.REVERSE
              animator.disableViewDuringAnimation(translateButton)
              animator.start()
          }

  3. Make the same change in rotater(), calling the new extension function:

          private fun rotater() {
              val animator = ObjectAnimator.ofFloat(star, View.ROTATION,
                                                    -360f, 0f)
              animator.duration = 1000
              animator.disableViewDuringAnimation(rotateButton)
              animator.start()
          }

________________________________________________________________________________


                              VIDEO 6

________________________________________________________________________________


Scale the Star

  Now you’re going to fill in the body of the scaler() function. This time,
  you’re going to animate two properties in parallel.

  In the previous two steps, you were just animating one property, because
  that’s all that was needed: rotating around a single axis (the “z” axis, which
  runs perpendicular to the screen) and translating along a single axis (the “x”
  axis, which runs left to right on the screen).

  But when an object is scaled, it is usually scaled in x and y simultaneously,
  to avoid making it look “stretched” along one of the axes (“fun-house mirror”
  is usually not the effect we strive for in UI design). So you should create an
  animation that will animate both the SCALE_X and SCALE_Y properties at the
  same time.

  Note: There is no single property that scales in both the x and y dimensions,
        so animations that scale in both x and y need to animate both of these
        separate properties in parallel.

  There are a couple of ways to do this (including using an AnimatorSet,
  which you will see in the final step of this lab). But a good technique
  to know about uses PropertyValuesHolder, which is an object that holds
  information about both a property and the values that that property
  should animate between.

  Note: A PropertyValuesHolder holds information about a single property,
        along the with the values that that property animates between.
        An ObjectAnimator can hold multiple PropertyValuesHolder objects, which
        will all animate together, in parallel, when the ObjectAnimator starts.
        The target that these PropertyValueHolder objects animate is specified
        by the ObjectAnimator. The ideal use case for ObjectAnimators which use
        PropertyValuesHolder parameters is when you need to animate several
        properties on the same object in parallel.

Step 1: Create an animation using PropertyValuesHolder

  In the previous tasks, you supplied information to ObjectAnimator about the
  property to be animated (such as TRANSLATE_X) along with the animation values
  (for example, the end value of 200f for translation). But you can instead use
  an intermediate object called PropertyValuesHolder to hold this information,
  and then create a single ObjectAnimator with multiple PropertyValuesHolder
  objects. This single animator will then run an animation on two or more of
  these sets of properties/values together.

  //TODO 1.11
  1. First, create two PropertyValuesHolder objects as the first lines
     in scaler():

          val scaleX = PropertyValuesHolder.ofFloat(View.SCALE_X, 4f)
          val scaleY = PropertyValuesHolder.ofFloat(View.SCALE_Y, 4f)

  Scaling to a value of 4f means the star will scale to 4 times its default size.

  Note: These PropertyValuesHolder objects look similar to the
        ObjectAnimators you created previously, but they only hold the property
        and value information for the animation, not the target. So even if you wanted to run these as an animation, you could not; there’s not enough information because you haven’t told the system which target object(s)
        to animate.

  //TODO 1.12
  2. Now create an ObjectAnimator object, as before, but use the scaleX and
     scaleY objects you created above to specify the property/value
     information:

          val animator = ObjectAnimator.ofPropertyValuesHolder(star, scaleX, scaleY)

  This is similar to the animators you created previously, but instead of
  defining a property and a set of values, it uses multiple
  PropertyValuesHolders which contain all of that information already. Using
  several PropertyValuesHolder objects in a single animator will cause them
  all to be animated in parallel.

Step 2: Cleaning up the animation

  Now you can complete the method as you did in previous tasks, resetting
  the object back to a reasonable end state and avoiding problems with
  discontinuous animations.

  //TODO 1.13
  1. As with the translater() function, you want to make this a
     repeating/reversing animation to leave the star’s SCALE_X and SCALE_Y
     properties at their default values (1.0) when the animation is done. Do
     this by setting the appropriate repeatCount and repeatMode values
     on the animator:

          animator.repeatCount = 1
          animator.repeatMode = ObjectAnimator.REVERSE

  //TODO 1.14
  2. Also, as with the previous animations, call the disableViewDurationAnimation()
     extension function to disable scaleButton during the animation. Adding in
     the rest of this code results in the final version of the function:

          private fun scaler() {
           val scaleX = PropertyValuesHolder.ofFloat(View.SCALE_X, 4f)
           val scaleY = PropertyValuesHolder.ofFloat(View.SCALE_Y, 4f)
           val animator = ObjectAnimator.ofPropertyValuesHolder(
                   star, scaleX, scaleY)
           animator.repeatCount = 1
           animator.repeatMode = ObjectAnimator.REVERSE
           animator.disableViewDuringAnimation(scaleButton)
           animator.start()
          }

  3. Run the application. Note that the star now scales out to 4x its original
     size… and then returns to its original state.


________________________________________________________________________________

                            THIRD COMMIT

________________________________________________________________________________

                                VIDEO 7


________________________________________________________________________________


Fade the Star

  Now for the final phase of the basic animations; you’re going to fade
  the star out to be completely transparent, and then back to fully opaque.

  Note: Fading items can be a very useful way to transition them into or
        out of a UI. For example, when removing an item from a list, you
        might fade out the contents first, before closing the gap that it
        leaves. Or if new information appears in a UI, you might fade it in.
        This effect not only avoids discontinuous motion, with UI elements
        snapping in and out in front of the user, but it helps alert the
        user that there is a change happening, instead of just removing
        or adding items and making them guess what just happened.

  Fading is done using the ALPHA property on View.

  Note: “alpha” is a term generally used, especially in computer graphics,
        to denote the amount of opacity in an object. A value of 0 indicates
        that the object is completely transparent, and a value of 1 indicates
        that the object is completely opaque. View objects have a default
        value of 1. Animations that fade views in or out animate the alpha
        value between 0 and 1.

  1. Define the fader() function to fade out the view to 0 and then back to
     its starting value. This code is essentially equivalent to the
     translater() function code you wrote before, except with a different
     property and end value. Here’s what the function looks like when it’s
     complete:

          private fun fader() {
              val animator = ObjectAnimator.ofFloat(star, View.ALPHA, 0f)
              animator.repeatCount = 1
              animator.repeatMode = ObjectAnimator.REVERSE
              animator.disableViewDuringAnimation(fade)
              animator.start()
          }


________________________________________________________________________________


                              VIDEO 8

________________________________________________________________________________


Colorizing

  One of the powerful things about ObjectAnimator, which might not be obvious
  from the examples so far, is that it can animate anything, as long as there
  is a property that the animator can access.

Step 1: Animating an arbitrary property

  Here is one simple example of animating a single property on an object.
  This time, that property isn’t an android.util.Property object, but is
  instead a property exposed via a setter, View.setBackgroundColor(int).
  Since you cannot refer to an android.util.Property object directly, like you
  did before with ALPHA, etc., you will use the approach of passing in the
  name of the property as a String. The name is then mapped internally to the
  appropriate setter/getter information on the target object.

  For this example, you will fill in the colorizer() function, which is
  called when you click on colorizerButton. In this animation, you will
  change the color of the star field background from black to red (and back).

  1. First, you will need an ObjectAnimator that can act on the appropriate
     type. You could use the ObjectAnimator.ofInt() factory method, since
     View.setBackgroundColor(int) takes an int, but… that would give us
     unexpected results. In the colorizer() function, create and run such
     an animator to see the problem:

          var animator = ObjectAnimator.ofInt(star.parent,"backgroundColor",
                                              Color.BLACK, Color.RED).start()

  2. Now run your application and click on colorizerButton. Isn’t that
     demo flashy? In fact, it’s a bit too flashy - it flashes between many
     different colors on the way from BLACK to RED. Without getting too much
     into the details of it, the problem is that the animation is
     interpreting raw integers as colors. Animating between two integer
     values does not necessarily yield the same result as animating between
     the colors that those two integers represent.

Step 2: Animate colors, not integers

  What you need, instead, is an animator that knows how to interpret (and
  animate between) color values, rather than simply the integers that
  represent those colors.

  1. Use a different factory method for the animator, ObjectAnimator.ofArgb().
     Try the code again, using this factory method instead:

          var animator = ObjectAnimator.ofArgb(star.parent,"backgroundColor",
                                                Color.BLACK, Color.RED).start()

  Now run the app. You’ll see that it smoothly animates from black to red,
  without those weird color flashes along the way.

  Note: The ofArgb() method is the reason that this app builds against
        minSdk 21; the rest of the functionality of the app can be run on
        earlier SDKs, but ofArgb() was introduced in the Lollipop release. It
        is also possible to animate color values on earlier releases, involving
        TypeEvaluators, and the use of ArgbEvaluator specifically. We used
        ofArgb() in this lesson instead for simplicity.

  The other thing to notice about this construction of the ObjectAnimator is
  the property: instead of specifying one of the View properties, like ALPHA,
  you are simply passing in the string “backgroundColor”. When you do this,
  the system searches for setters and getters with that exact spelling using
  reflection. It caches references to those methods and calls them during the
  animation, instead of calling the Property set/get functions as the previous
  animations did.

Step 3: Fade [back] to black

  Anyway, back to our animation. We currently have the ability to animate from
  black to red… and that’s where it stays. If you click the button again, it
  will animate again, but it always ends up at red, because the animation
  explicitly animates from black to and end value of red.

  1. Change the animation to take a little longer to run, by setting an
     explicit duration, and then animate back to black. You should also disable
     the button during the animation, as you did with the other animations,
     by calling the extension function created earlier. Here’s what that
     complete function looks like:

          private fun colorizer() {
           var animator = ObjectAnimator.ofArgb(star.parent,
               "backgroundColor", Color.BLACK, Color.RED)
           animator.setDuration(500)
           animator.repeatCount = 1
           animator.repeatMode = ObjectAnimator.REVERSE
           animator.disableViewDuringAnimation(bgColor)
           animator.start()
          }

  That's all there is to it. This animation is very similar to all of the rest
  you've set up in this lab, except for the use of the “backgroundColor”
  string for the property name. This doesn’t seem all that different from
  what you did before, except that it means you can use ObjectAnimator to
  animate literally anything that has a setter/getter. For example, you could
  have a custom View with a property called lineLength, that sets the length
  of some line segment in your UI (maybe using custom drawing code in an
  onDraw()) override). Passing in “lineLength” to the animator constructor
  would result in animating that line length, because the system maps that
  string to the underlying property setter in your custom view code.

  You can, and should, use ObjectAnimator for all property animations in
  your application. There are other kinds of animations you can create in
  applications (including whole-application animation choreography, using
  MotionLayout, but for individual property animations, ObjectAnimator
  is the way to go.

________________________________________________________________________________


                              VIDEO 9

________________________________________________________________________________


Star Shower

  Now for the final step, you will create a slightly more involved animation,
  animating multiple properties on multiple objects.

  For this effect, a button click will result in the creation of a star with
  a random size, which will be added to the background container, just out of
  view of the top of that container. The star will proceed to fall down to
  the bottom of the screen, accelerating as it goes. As it falls, it
  will rotate.

  For this step, you will fill in the shower() function, to wire up a single
  animation of a falling star to a single click of the SHOWER button. There
  are a few new concepts here, in addition to things you’ve seen in
  the previous steps.

Step 1: A Star is Born

  First, you’re going to need some local variables to hold state that we
  will need in the ensuing code. Specifically, you’ll need:

  * a reference to the star field ViewGroup (which is just the parent of
    the current star view).

  * the width and height of that container (which you will use to calculate
    the end translation values for our falling stars).

  * the default width and height of our star (which you will later alter
    with a scale factor to get different-sized stars).


  1. Start filling out the inside of the shower() function. Add this code:

          val container = star.parent as ViewGroup
          val containerW = container.width
          val containerH = container.height
          var starW: Float = star.width.toFloat()
          var starH: Float = star.height.toFloat()

  2. Create a new View to hold the star graphic. Because the star is a
     VectorDrawable asset, use an AppCompatImageView, which has the ability
     to host that kind of resource. Create the star and add it to the
     background container.

          val newStar = AppCompatImageView(this)
          newStar.setImageResource(R.drawable.ic_star)
          newStar.layoutParams = FrameLayout.LayoutParams(FrameLayout.LayoutParams.WRAP_CONTENT,
                                 FrameLayout.LayoutParams.WRAP_CONTENT)
          container.addView(newStar)

  3. Run the app. Click on the SHOWER button. You will see the new star you
     created in the top-left corner.

     https://video.udacity-data.com/topher/2019/October/5d94d65a_image6/image6.png

Step 2: Sizing and positioning the star

  You haven’t yet told this image where to be positioned in the container,
  so it’s positioned at (0, 0) by default. You will fix this placement
  in this step.

  1. Set the size of the star. Modify the star to have a random size, from
     .1x to 1.6x of its default size. Use this scale factor to change the
     cached width/height values, because you will need to know the actual
     pixel height/width for later calculations.

          newStar.scaleX = Math.random().toFloat() * 1.5f + .1f
          newStar.scaleY = newStar.scaleX
          starW *= newStar.scaleX
          starH *= newStar.scaleY

  You have now cached the star’s pixel H/W stored in starW and starH:
  https://video.udacity-data.com/topher/2019/October/5d94d6c1_image4/image4.png


  2. Now position the new star. Horizontally, it should appear randomly
     somewhere from the left edge to the right edge. This code uses the width
     of the star to position it from half-way off the screen on the left
     (-starW / 2) to half-way off the screen on the right (with the star
     positioned at (containerW - starW / 2). The vertical positioning of the
     star will be handled later in the actual animation code.

          newStar.translationX = Math.random().toFloat() *
                                 containerW - starW / 2

    https://video.udacity-data.com/topher/2019/October/5d94d710_image1/image1.png


Step 3: Creating animators for star rotation and falling

  You’re done setting up the initial star information; it’s time to work on
  the animation. The star should rotate as it falls downwards. You’ve already
  seen one way to animate two properties together, using PropertyValuesHolder,
  in the previous task on scaling. You could do a similar thing here, except
  there will be different types of motion, what we call “interpolation,” on
  these two animations. Specifically, the rotation will use a smooth linear
  motion (moving at a constant rate over the entire rotation animation), while
  the falling animation will use an accelerating motion (simulating gravity
  pulling the star downward at a constantly faster rate). So you'll create
  two animators and add an interpolator to each..

  1. First, create two animators, along with their interpolators:

          val mover = ObjectAnimator.ofFloat(newStar, View.TRANSLATION_Y,
                                             -starH, containerH + starH)
          mover.interpolator = AccelerateInterpolator(1f)
          val rotator = ObjectAnimator.ofFloat(newStar, View.ROTATION,
                        (Math.random() * 1080).toFloat())
          rotator.interpolator = LinearInterpolator()

  The mover animation is responsible for making the star “fall.” It animates
  the TRANSLATION_Y property, similar to what you did with TRANSLATION_X
  in the earlier translation task, but causing vertical instead of
  horizontal motion. The code animates from -starH to (containerH + starH),
  which effectively places it just off the container at the top and moves it
  until it’s just outside the container at the bottom, as shown here:

  https://video.udacity-data.com/topher/2019/October/5d94d7c1_image2/image2.png

  The AccelerateInterpolator “interpolator” that we are setting on the star
  causes a gentle acceleration motion.

  For the rotation animation, the star will rotate a random amount between
  0 and 1080 degrees (three times around). For the motion, we are simply
  using a LinearInterpolator, so the rotation will proceed at a constant
  rate as the star falls.

  Note: There are several interpolators in the Android system, some more
        powerful and flexible than others, such as PathInterpolator.
        You can experiment with them to see which ones you like for your
        UI animations.

Step 4: Running the animations in parallel with AnimatorSet

  Now it is time to put these two animators together into a single AnimatorSet,
  which is useful for this slightly more complex animation involving multiple
  ObjectAnimators. AnimatorSet is basically a group of animations, along with
  instructions on when to run those animations. It can play animations in
  parallel, as you will do here, or sequentially (like you might do in
  the list-fading example mentioned earlier, where you first fade out
  a view and then animate the resulting gap closed). An AnimatorSet can
  also contain other AnimatorSets, so you can create very complex
  hierarchical choreography by grouping animators together into these sets.

  1. Create the AnimatorSet and add the child animators to it (along with information to play them in parallel). The default animation time of 300 milliseconds is too quick to enjoy the falling stars, so set the duration to a random number between 500 and 2000 milliseconds, so stars fall at different speeds.

          val set = AnimatorSet()
          set.playTogether(mover, rotator)
          set.duration = (Math.random() * 1500 + 500).toLong()

  2. Once newStar has fallen off the bottom of the screen, it should be removed
     from the container. Set a simple listener to wait for the end of the
     animation and remove it. Then start the animation.

  Remember: It should seem obvious that when a view is not needed anymore,
  it should be removed. But complex animations can make us forget about
  the simple things. Use animation listeners to handle important
  tasks for bringing views onto or off of the screen, like we’re
  doing here, to remove a view when the animation to move it off
  the screen is complete.

          set.addListener(object : AnimatorListenerAdapter() {
              override fun onAnimationEnd(animation: Animator?) {
                  container.removeView(newStar)
              }
          })
          set.start()

  3. Run your application. You can click on the SHOWER button multiple times,
     creating a new star and new animation each time. Note that you didn’t
     have to disable    the   button during the animation, as you did in
     the earlier tasks, because this time we wanted to create several
     simultaneous animations. There was no problem with discontinuous
     motion artifacts because each animation is independent of the
     others and operates on a different target object.

You should see something like this:

  https://video.udacity-data.com/topher/2019/October/5d94d90f_image5/image5.png


Congratulations

  Congratulations, you've successfully built an app that runs several
  different kinds of property animations. Animating stars may not be
  the kind of UI experience you want in your applications, but the
  tools you used in this lab are exactly the tools you should use
  to animate UI elements in real-world situations.
  ObjectAnimator, AnimatorSet, LinearInterpolator, PropertyValuesHolder;
  these are all good APIs to understand in order to write animations
  in your code.

________________________________________________________________________________
________________________________________________________________________________
