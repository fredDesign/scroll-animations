http://www.oxygenna.com/tutorials/scroll-animations-using-waypoints-js-animate-css


# scroll-animations
on scroll animations tutorial
On scroll animation is a technique which has gained popularity over the past several months. It suits one-page websites as well as websites with many illustrations perfectly, giving them a more dynamic appearance. However, the animations can be applied to any element as we will soon see.

For this tutorial we assume the reader has some prior experience with javascript and more specifically with the jQuery library, as we will need the aid of the popular jQuery-waypoints plugin. Waypoints is a plugin that allows us to easily execute a function whenever we scroll to an element.

The following example will alert the user when he scrolls on any element that has a class ‘notify’:
1
2
3
	
$('.notify').waypoint(function(direction) {
  alert('Top of notify element hit top of viewport.');
});

One can pass a second argument to the waypoint function, which is an options object. An option of interest for us today is the offset option, which tells Waypoints how far from the top of the window the callback function should fire.
1
2
3
4
5
	
$('.notify').waypoint(function(direction) {
  alert('Top of notify element hit top of viewport.');
},{
  offset:'90%'
});

In the example above we express the offset as a percentage of the viewport’s height.

Having a solid grasp of how Waypoints work, we can now move on to Daniel Eden’s animate.css library. animate.css is a bunch of cool fun and cross-browser animations that we can use in our projects.

Grab a copy of the stylesheet and include it in your project. After that you will be able to animate your elements by simply adding the class animated to them, along with the name of the specific animation you want to apply.We will use jQuery to add the animation classes to our elements.The following code illustrates how this can be achieved:
1
	
$('.toBeAnimated').addClass('animated fadeInLeft');

The above code will make all elements that have a class toBeAnimated to animate

animate.css also let’s us modify the duration of our animations and add a delay which can come in handy.
1
2
3
4
	
.toBeAnimated {
  -vendor-animation-duration: 3s;
  -vendor-animation-delay: 2s;
}

The above style will trigger an animation that lasts 3 seconds, 2 seconds after the class animated is applied to an element.

The best way to define the animations (or behaviours in general) for an element would be through data attributes.We will use data attributes to specify what type of animations our elements will perform, as well as possible delays in the animation. Here is what the markup for an animated section will look like:
1
2
3
4
	
<section class="os-animation" data-os-animation="swing" data-os-animation-delay="0s">
    <h1>This section will swing</h1>
    <p>Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
</section>

Studying the snipped above, one will notice that we added a class os-animation to our section. That will help us target this as well as other sections with the same class through jQuery. The two data attributes specify the animation we want the section to perform, as well as the delay (no delay in this case).

Wouldn’t it be great if our section was initially hidden and only appeared when it scrolled into view and animated? Let’s add some css that will help us accomplish exactly that:
1
2
3
4
5
6
7
	
.os-animation{
  opacity: 0;
}
 
.os-animation.animated{
    opacity: 1;
}

It is now time to combine the two concepts we saw earlier and create a small script that will handle the animation of our elements once they come into view. Without further ado:
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
19
20
21
22
	
function onScrollInit( items, trigger ) {
  items.each( function() {
    var osElement = $(this),
        osAnimationClass = osElement.attr('data-os-animation'),
        osAnimationDelay = osElement.attr('data-os-animation-delay');
 
    osElement.css({
        '-webkit-animation-delay':  osAnimationDelay,
        '-moz-animation-delay':     osAnimationDelay,
        'animation-delay':          osAnimationDelay
    });
 
    var osTrigger = ( trigger ) ? trigger : osElement;
 
    osTrigger.waypoint(function() {
        osElement.addClass('animated').addClass(osAnimationClass);
    },{
        triggerOnce: true,
        offset: '90%'
    });
  });
}

The above function expects two arguments, the second being optional. A list of items that we want to animate, and an optional container in case we want to perform staggered animations. For each of those elements, we extract the animation information from their data attributes, and animate them once they appear at the bottom of our viewport. Notice that we added the triggerOnce option in our options object since we want each element to animate only once, when it comes into our view as we scroll.

Calling the above function once the document has loaded with the following code:
1
	
onScrollInit( $('.os-animation') );

will animate our previous section as well as other sections with the same class and similar data attributes once they come into our view.

But what if I want staggered animations? An animation is called staggered when it contains a delay between each successive animation. You have most probably seen it in websites that animate paragraphs that belong in the same section in ‘layers’. Let’s see how we can use our previous function in order to achieve that as well. Create a section in your body of your html document that looks like this:
1
2
3
4
5
6
	
<section class="staggered-animation-container">
    <h1>This section contains staggered animations!</h1>
    <p class="staggered-animation" data-os-animation="fadeInRight" data-os-animation-delay="0.5s">Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
    <p class="staggered-animation" data-os-animation="fadeInRight" data-os-animation-delay="0.8s">Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
    <p class="staggered-animation" data-os-animation="fadeInRight" data-os-animation-delay="1.1s">Lorem ipsum dolor sit amet, consectetur adipisicing elit.</p>
</section>

In the above code you will notice that we apply the animation attributes to the paragraphs inside the section, which all have the same class staggered-animation and we add a staggered-animation-container class to the section that contains them. The idea this time is to trigger the waypoint callback once the section is at the bottom of our view, and have the paragraphs inside animate in layers.Thus we will call our function this time like this:
1
	
onScrollInit( $('.staggered-animation'), $('.staggered-animation-container') );

This time we passed a second argument to the function as well. The first argument is the selector for the elements we want to animate, and the second is for their container that will trigger the animations once it scrolls into view. Notice in the markup that we needed different (incemental) delays for each item inside the container in order to achieve the effect.


