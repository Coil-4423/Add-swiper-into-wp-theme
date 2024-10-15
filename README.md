To add **Swiper.js** into a WordPress theme, you'll need to follow a few steps to properly enqueue the Swiper CSS and JavaScript, initialize the Swiper instance, and insert it into your template files.

Here’s a detailed guide:

### 1. Enqueue Swiper.js CSS and JavaScript
First, enqueue the necessary Swiper.js files (both CSS and JavaScript) by adding them to your `functions.php` file.

In your `functions.php` file, add the following code:

```php
function enqueue_swiper_assets() {
    // Enqueue Swiper CSS
    wp_enqueue_style( 'swiper-css', 'https://cdn.jsdelivr.net/npm/swiper/swiper-bundle.min.css' );
    
    // Enqueue Swiper JS
    wp_enqueue_script( 'swiper-js', 'https://cdn.jsdelivr.net/npm/swiper/swiper-bundle.min.js', array(), null, true );
    
    // Custom script to initialize Swiper (optional if you want to keep the JS in a separate file)
    wp_add_inline_script('swiper-js', '
        var swiper = new Swiper(".swiper-container", {
            slidesPerView: 1,
            spaceBetween: 10,
            pagination: {
                el: ".swiper-pagination",
                clickable: true,
            },
            navigation: {
                nextEl: ".swiper-button-next",
                prevEl: ".swiper-button-prev",
            },
        });
    ');
}
add_action( 'wp_enqueue_scripts', 'enqueue_swiper_assets' );
```

### 2. Add Swiper Markup in Template File
Next, in the template file where you want to display the Swiper (for example, `page.php`, `single.php`, or your custom template), you need to add the Swiper HTML structure.

Here’s an example of Swiper's basic markup:

```php
<div class="swiper-container">
    <div class="swiper-wrapper">
        <div class="swiper-slide">Slide 1</div>
        <div class="swiper-slide">Slide 2</div>
        <div class="swiper-slide">Slide 3</div>
        <!-- Add more slides here -->
    </div>
    <!-- Add Pagination -->
    <div class="swiper-pagination"></div>
    
    <!-- Add Navigation -->
    <div class="swiper-button-next"></div>
    <div class="swiper-button-prev"></div>
</div>
```

This is the basic structure for Swiper. Each `.swiper-slide` represents a slide.

### 3. Customize Swiper Initialization (Optional)
If you want to have more control over the Swiper instance, you can move the initialization script into a separate JavaScript file and enqueue it in your theme. For example:

1. Create a file named `swiper-init.js` inside your theme’s `js` folder (e.g., `wp-content/themes/your-theme/js/swiper-init.js`).

2. Add the following Swiper initialization code to that file:

```javascript
document.addEventListener('DOMContentLoaded', function () {
    var swiper = new Swiper('.swiper-container', {
        slidesPerView: 1,
        spaceBetween: 10,
        pagination: {
            el: '.swiper-pagination',
            clickable: true,
        },
        navigation: {
            nextEl: '.swiper-button-next',
            prevEl: '.swiper-button-prev',
        },
        loop: true, // Enable looping
        autoplay: {
            delay: 5000, // 5 seconds delay
        },
    });
});
```

3. Update your `functions.php` to enqueue this custom script:

```php
function enqueue_swiper_assets() {
    // Enqueue Swiper CSS
    wp_enqueue_style( 'swiper-css', 'https://cdn.jsdelivr.net/npm/swiper/swiper-bundle.min.css' );
    
    // Enqueue Swiper JS
    wp_enqueue_script( 'swiper-js', 'https://cdn.jsdelivr.net/npm/swiper/swiper-bundle.min.js', array(), null, true );

    // Enqueue your custom Swiper initialization script
    wp_enqueue_script( 'swiper-init', get_template_directory_uri() . '/js/swiper-init.js', array('swiper-js'), null, true );
}
add_action( 'wp_enqueue_scripts', 'enqueue_swiper_assets' );
```

### 4. Style the Swiper (Optional)
You may want to customize the appearance of your Swiper slides using CSS. Add your custom styles to your theme’s stylesheet (or a separate CSS file). Here’s a basic example:

```css
.swiper-container {
    width: 100%;
    height: 400px;
}

.swiper-slide {
    background-color: #f3f3f3;
    display: flex;
    justify-content: center;
    align-items: center;
    font-size: 24px;
}

.swiper-button-next, .swiper-button-prev {
    color: #000; /* Customize the color of navigation arrows */
}
```

### 5. Optional: Use Swiper with ACF (Advanced Custom Fields)
If you want to use Swiper to dynamically display images or content from ACF fields, you can loop through the ACF fields and generate the slides.

Here’s an example that assumes you have an ACF repeater field called `slider_images`:

```php
<?php if( have_rows('slider_images') ): ?>
<div class="swiper-container">
    <div class="swiper-wrapper">
        <?php while( have_rows('slider_images') ): the_row(); 
            $image = get_sub_field('image'); ?>
            <div class="swiper-slide">
                <img src="<?php echo esc_url($image['url']); ?>" alt="<?php echo esc_attr($image['alt']); ?>" />
            </div>
        <?php endwhile; ?>
    </div>
    <!-- Add Pagination -->
    <div class="swiper-pagination"></div>
    
    <!-- Add Navigation -->
    <div class="swiper-button-next"></div>
    <div class="swiper-button-prev"></div>
</div>
<?php endif; ?>
```

This example dynamically generates the Swiper slides based on the images in the ACF repeater field.

### Conclusion:
You now have Swiper.js integrated into your WordPress theme. You can customize the initialization and appearance of the slider according to your needs, and if you're using ACF, you can easily loop through custom fields to generate dynamic Swiper slides.
