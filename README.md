# Project - Wordpress with external post creator
 
##Create WordPress Post from external site 

![Creating WordPress posts from external websites](https://photos.app.goo.gl/sF4iRDuyZcRFL2j59)


Wordpres site provides many useful wp functions:

> wp_remote_post
> wp_update_post
> wp_mkdir_p
> wp_insert_attachment
> wp_generate_attachment_metadata
> wp_update_attachment_metadata
> set_post_thumbnail
> etc

###### Instruction to integrating 

Follw this instruction:
> Install Application Passwords, JSON Authentication Master WP plugins
> Create CRUD (Create, Read, Update, Delete) php files seperately in WP root folder
> Include wp_load.php file in each php file
> Use wp functions

```php
$api_response = wp_remote_post( '{domain}/wp-json/wp/v2/posts', array(
 'sslverify' => FALSE,
 'headers' => array( 'Authorization' => 'Basic ' . base64_encode( '{username}:{password} ')),
 'body' => array( 
          'title'   => 'Test Post',
          'status'  => 'draft', // publish, draft, future,etc, -> post status
          'content' => 'Test Content',//post content
          'categories' => 2, // category ID
          'tags' => '2,3,4' // string, comma separated
          'date' => $date, // YYYY-MM-DDTHH:MM:SS
          'excerpt' => 'Read this awesome post',
          'password' => '12$45',
          'slug' => 'test_post',//str_replace(' ','_',$subject) // part of the URL usually
        )
  ));

$body = json_decode( $api_response['body'] );
// you can always print_r to look what is inside
// print_r( $body ); // or print_r( $api_response );
if( wp_remote_retrieve_response_message( $api_response ) === 'Created' ) {
 echo $body->id;//$lastid = $wpdb->insert_id;
}
```

Follw this instruction to update featured image of post:
> Get post id
> Update post featued image with metadata changes.

```php
// Set attachment data
$attachment = array(
 'post_mime_type' => $wp_filetype['type'],
 'post_title'     => sanitize_file_name( $filename ),
 'post_content'   => '',
 'post_status'    => 'inherit'
);
// Create the attachment
$attach_id = wp_insert_attachment( $attachment, $file, $post_id );
// Include image.php
require_once(ABSPATH . 'wp-admin/includes/image.php');
// Define attachment metadata
$attach_data = wp_generate_attachment_metadata( $attach_id, $file );
// Assign metadata to attachment
wp_update_attachment_metadata( $attach_id, $attach_data );
// And finally assign featured image to post
set_post_thumbnail( $post_id, $attach_id );
```



