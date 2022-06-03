# LiveProjectCodeSummary

# Introduction
While studying at the Tech Academy I had the oppurtunity to take part in a two week sprint, I worked in a team developing a full scale MVC Web Application in C#. Working on a legacy codebase was a great learning experience for cleaning up code, and adding requested features. Because much of the site had already been built, there were a multitude of [front end stories](#Front-End-Stories) and UX improvements that needed to be completed. I worked on one [major story](#Image-to-Byte-Array) in particular that involved both front end UI work and back end work with databases. I am very proud of. Over the two week sprint I also had the opportunity to hone my project management skills that I'm confident I will use again and again on future projects.

Below are descriptions of the stories I worked on, along with code snippets and navigation links. I also have some full code files in this repo for the larger functionalities I implemented.



# Image to Byte Array

  * [Byte Array Conversion Method in Controller](#Byte-Array-Conversion-Method-in-Controller)
  * [Displaying Byte Array to an Image](#Displaying-Byte-Array-to-an-Image)



  ### Byte Array Conversion Method in Controller
  
  I was tasked with adding the ability for a user to upload an image and have said image stored in the web applications database as a byte array and would then display   it back as an image on the corresponding pages (Index, Edit, Delete pages). To do this I first made a new method within the controller that accepted a posted file     and converted it to byte array.  
  
  
  Taken from Blog Photos Controller
  
      public byte[] BlogPhotoByte(HttpPostedFileBase postedFile)
      {
          byte[] bytes;
          using (BinaryReader br = new BinaryReader(postedFile.InputStream))
          {
              bytes = br.ReadBytes(postedFile.ContentLength);
          }
          return bytes;
      }


    
  Next I took the data converted from BlogPhotoByte and added that into the Create method in the controller.
  
  
  *From Blog Photos Controller*
  
      public ActionResult Create([Bind(Include = "BlogPhotoId,Title,Photo")] BlogPhoto blogPhoto, HttpPostedFileBase postedFile)
      {
          if (ModelState.IsValid)
          {               
              var photoByte = BlogPhotoByte(postedFile);
              blogPhoto.Photo = photoByte;
              db.BlogPhotos.Add(blogPhoto);
              db.SaveChanges();
              return RedirectToAction("Index");
          }

          return View(blogPhoto);
      }
  
  
  
  ### Displaying Byte Array to an Image
    
    
  With the user uploaded image converted to a byte array and stored in a database on the back end I then had to convert the byte array back to an image using Base64String and display it on the corresponding page. First I needed to delcare a variable for the byte array to instantiate it, next within the HTML img tag using razor syntax added the converted image to be displayed.
  
  *From Blog Index Page*
    
      <div class="card-columns blogphoto-myCardColumns--column">
          @foreach (var item in Model)
          {
              var byteImage = Convert.ToBase64String(item.Photo);

              <div class="card blogphoto-card--card">
                  <a class="blogphoto-cardDetails--action" href="@Url.Action("Details", "BlogPhotoes", new { id = item.BlogPhotoId })"><img src="data:image/jpg;base64, @byteImage" class="card-img-top blogphoto-cardImage--image" alt="Blog Photo failed to load"></a>
                  <p class="blogphoto-cardText--text">
                      @Html.DisplayFor(modelItem => item.Title)
                  </p>
                  <p class="blogphoto-cardButtons--btn">
                      @Html.ActionLink("Edit", "Edit", new { id = item.BlogPhotoId }, new { @class = "btn blogphoto-edit-index--btn" })
                      @Html.ActionLink("Delete", "Delete", new { id = item.BlogPhotoId }, new { @class = "btn blogphoto-delete--btn" })
                  </p>
              </div>
           }
      </div>      
            



# Front End Stories

  * [Index](#Index-Page)
  * [Create](#Create-Page)
  * [Details](#Details-Page)
  * [Delete](#Delete-Page)
  
  
  ### Styling web pages
  
  Most styling of the web pages consisted of matching the web pages theme to the input fields and buttons and also adjusting the postioning and spacing of the elements   in the web page. Below are side by side screenshots of the webpage before and after showing the styling changes and [displays the user uploaded photo](#Image-to-Byte-Array)
  
  
  #### Index Page
    * Title and buttons is now apart of each image displayed, only appears when mouse hovers over a specific image
    * Layout was made to resemble a Masonry layout 
  ![Untitled-Artwork 3](https://user-images.githubusercontent.com/101282383/171802408-a1d5781e-b923-4917-8d78-3ee38cf4fad0.png)


  #### Create Page
  ![Untitled-Artwork 2](https://user-images.githubusercontent.com/101282383/171802419-3cff9e30-06d1-4395-afe8-52925bf53a36.png)


  #### Details Page
  ![Untitled-Artwork 1](https://user-images.githubusercontent.com/101282383/171802425-1f604b2a-3139-45d8-9958-f3337779e47e.png)


  #### Delete Page
  ![Untitled-Artwork](https://user-images.githubusercontent.com/101282383/171802431-680f08c3-c422-49f5-b520-4224c04e7307.png)


