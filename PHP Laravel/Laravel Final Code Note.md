## 1. Java Script Notification Code:

1  Add link in the head of html template and Download this link and script from website
```html
<link href="{{asset('css/toastify.min.css')}}" rel="stylesheet" />
<script src="{{asset('js/jquery-3.7.0.min.js')}}"></script>
<script src="{{asset('js/toastify-js.js')}}"></script>
```
2  Add this code in config.js ( Success and Error Toast  JavaScript code )
```js code 
// This is Rabbil vai code
function successToast(msg) {  
    Toastify({  
	    text: msg,  
        gravity: "bottom", // `top` or `bottom`  
        position: "center", // `left`, `center` or `right`  
        className: "mb-5",  // error box show from bleow how distance
        style: {  
            background: "green",  
        }  
    }).showToast();  
}  
function errorToast(msg) {  
    Toastify({  
	    text: msg, 
        gravity: "bottom", // `top` or `bottom`  
        position: "center", // `left`, `center` or `right`  
        className: "mb-5",  
        style: {  
            background: "red",  
        }  
    }).showToast();  
}


or
function successToast(msg) {  
    Toastify({  
		text: msg,  
        gravity: "bottom", // `top` or `bottom`  
        position: "center", // `left`, `center` or `right`  
        className: "mb-5",  
        style: {  
            background: "green",  
            color: "white", 
            borderRadius: "5px", 
            padding: "10px 20px", 
            fontSize: "14px", 
            zIndex: 99999,
        }  
    }).showToast();  
}  
function errorToast(msg) {
	Toastify({
		text: msg,
		gravity: "bottom", 
		position: "center",
		className: "mb-5",
		style: {
			background: "red",
			color: "white",
			borderRadius: "5px",
			padding: "10px 20px",
			fontSize: "14px",
			zIndex: 99999,
		},
	}).showToast();
	}
```
3  Use code in the script tag
```JavaScript
let categoryName = document.getElementById('categoryName').value;  
let categoryPrice = document.getElementById('categoryPrice').value;

if (categoryName.length === 0) {  
    errorToast("Category Name is Required !")  
}else if(categoryPrice.length === 0){
	errorToast("Category Price is Required !")  
}else{
	write others code here
}
```

## 2. Laravel Notification Code
```js
// link gulo add koro
<script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/2.1.4/toastr.min.js" integrity="sha512-lbwH47l/tPXJYG9AcFNoJaTMhGvYWhVM9YI43CT+uteTRRaiLCui8snIgyAN8XWgNjNhCqlAUdzZptso6OCoFQ==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>  
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/2.1.4/toastr.min.css" integrity="sha512-6S2HWzVFxruDlZxI3sXOZZ4/eJ8AcxkQH1+JjSe/ONCEqR9L4Ysq5JdT5ipqtzU7WHalNwzwBv+iE51gNHJNqQ==" crossorigin="anonymous" referrerpolicy="no-referrer" />

// <body></body> tag ar vetor likhte paro ba onno jekono jaygay not sure try kore dekho 
@if(Session::has('success'))  
    <script>  
        toastr.options = {  
            "progressBar": true,  
            "closeButton": true,  
        }  
        toastr.success("{{ Session::get('success') }}", 'Success!', {timeOut: 2000});  
    </script>  
@endif  
@if(Session::has('error'))  
    <script>  
        toastr.options = {  
            "progressBar": true,  
            "closeButton": true,  
        }  
        toastr.error("{{ Session::get('error') }}", 'Error!', {timeOut: 2000});  
    </script>  
@endif
```
## 3. JavaScript Show Loader and Hide Loader

1  Add this in view blade file  
```JavaScript
<script>  
    async function Save() {  
        let categoryName = document.getElementById('categoryName').value;  
        if (categoryName.length === 0) {  
            errorToast("Category Required !")  
        }  
        else {  
            document.getElementById('modal-close').click();  
            showLoader();  
            let res = await axios.post("/create-category",{name:categoryName})  
            hideLoader();  
            if(res.status===201){  
  
                successToast('Request completed');  
  
                document.getElementById("save-form").reset();  
  
                await getList();  
            }  
            else{  
                errorToast("Request fail !")  
            }  
        }  
    }  
</script>
```
2  Add this in config.js file
```JavaScript
function showLoader() {  
    document.getElementById('loader').classList.remove('d-none')  
}  
function hideLoader() {  
    document.getElementById('loader').classList.add('d-none')  
}
```

## 4. Validation Code 

```JavaScript 
if(!checkValidation()){  // this means false
    return; // here return; means stop this code here and according to this function this will not stop here as checkvalidation return true we know it from the below function. If return flase then go to inside the function and give return and stop here. 
}
function checkValidation() {  
    let projectName = document.getElementById('projectName').value;  
    if (projectName.trim().length === 0) {  
        errorToast('Project Name is required');  
    } else {  
        return true;  
    }  
}
```

## 5. Dropdown list with image code

```Html & JavaScript
// link
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/css/bootstrap.min.css" rel="stylesheet">
<link href="https://cdn.jsdelivr.net/npm/select2@4.1.0/dist/css/select2.min.css" rel="stylesheet">
//script
<script src="https://cdn.jsdelivr.net/npm/jquery@3.6.4/dist/jquery.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/bootstrap@5.3.0/dist/js/bootstrap.bundle.min.js"></script>
<script src="https://cdn.jsdelivr.net/npm/select2@4.1.0/dist/js/select2.min.js"></script>



{{-- Html code Start--}}  
<div class="col-md-4 align-items-center">  
    <label class="control-label">Select Your Brand </label>  
    <select name="brand_name" id="brand_name" class="form-select" aria-label="Default select example" style="padding: 8px;margin-bottom: 11px;">  
        <option value="">&nbsp;&nbsp;All Brands List</option>  
        @foreach($brands as $brand)  
            <option value="{{$brand->id}}" data-image="{{asset( $brand->logo) }}">{{ $brand->name }}</option>  
        @endforeach  
    </select>  
</div>  
{{-- Html code End --}}  
  
  
//JavaScript code Start  
<script>  
    $(document).ready(function () {  
        $('#brand_name').select2({  
            templateResult: formatOption,  
            templateSelection: formatOption,  
        });  
        function formatOption(option) {  
            if (!option.id) {  
                return option.text;  
            }  
            const imgUrl = $(option.element).data('image');  
            return $(`<span class="custom-option"><img src="${imgUrl}" alt="icon" />${option.text}</span>`);  
        }  
    });  
</script>  
//JavaScript code End

```

## 6. Modal Form fill up

 ```html
// link start
<link href="{{asset('css/bootstrap.css')}}" rel="stylesheet" />  
<link href="{{asset('css/animate.min.css')}}" rel="stylesheet" />
<script src="{{asset('js/bootstrap.bundle.js')}}"></script>


// html code start
<button data-bs-toggle="modal" data-bs-target="#create-modal" class="float-end btn m-0  bg-gradient-primary">Create</button>  
  
<div class="modal animated zoomIn modal-xl smooth-madal fade" id="create-modal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
// fade dile slowly open hoy
    <div class="modal-dialog modal-lg modal-dialog-centered">  
        <div class="modal-content">  
            
            <div class="modal-header">  
                <h5 class="modal-title" id="exampleModalLabel">Create Product</h5>  
            </div>  
                      
            <div class="modal-body">  
                <form id="save-form">  
                    <div class="container">  
                        <div class="row">  
                            <div class="col-12 p-1">  
  
                                <label class="form-label">Category</label>  
                                <select type="text" class="form-control form-select" id="productCategory">  
                                    <option value="">Select Category</option>  
                                </select>  
                                
                                <label class="form-label mt-2">Name</label>  
                                <input type="text" class="form-control" id="productName">  
  
                                <label class="form-label mt-2">Price</label>  
                                <input type="text" class="form-control" id="productPrice">  
  
                                <label class="form-label mt-2">Unit</label>  
                                <input type="text" class="form-control" id="productUnit">  
                                <br/>
                                <img class="w-10" id="newImg" src="{{asset('images/default.jpg')}}"/>  
                                <br/>  
                                
                                <label class="form-label">Image</label>  
                                <input oninput = "newImg.src = window.URL.createObjectURL(this.files[0])" type="file" class="form-control" id="productImg">  
  
                            </div>
                        </div>                    
                    </div>                
                </form>            
            </div>            
                            
	        <div class="modal-footer">  
		        <button id="modal-close" class="btn bg-gradient-primary mx-2" data-bs-dismiss="modal" aria-label="Close">Close</button>  
		        <button onclick="Save()" id="save-btn" class="btn bg-gradient-success" >Save</button>          
		    </div>
		    
        </div>
    </div>
</div>

// html code end
```
## 7. Delete Modal 
```html
// link
<link href="{{asset('css/bootstrap.css')}}" rel="stylesheet" />  
<link href="{{asset('css/animate.min.css')}}" rel="stylesheet" />
<link href="{{asset('css/style.css')}}" rel="stylesheet" />

<script src="{{asset('js/jquery-3.7.0.min.js')}}"></script>
<script src="{{asset('js/axios.min.js')}}"></script>
<script src="{{asset('js/bootstrap.bundle.js')}}"></script>

// html start
  
<button data-path="${item['img_url']}" data-id="${item['id']}" class="btn deleteBtn btn-sm btn-outline-danger">Delete</button>  

<div class="modal animated zoomIn" id="delete-modal">  
    <div class="modal-dialog modal-dialog-centered">  
        <div class="modal-content"> 
            <div class="modal-body text-center">  
                <h3 class=" mt-3 text-warning">Delete !</h3>  
                <p class="mb-3">Once delete, you can't get it back.</p>  
                <input class="d-none" id="deleteID"/>  
                <input class="d-none" id="deleteFilePath"/>  
            </div>      
                  
            <div class="modal-footer justify-content-end">  
                <div>
	                <button type="button" id="delete-modal-close" class="btn bg-gradient-success mx-2" data-bs-dismiss="modal">Cancel</button>  
	                <button onclick="itemDelete()" type="button" id="confirmDelete" class="btn bg-gradient-danger" >Delete</button>  
                </div>
            </div>
        </div>
    </div>
</div>
// html end


// script start
<script>  
    $('.deleteBtn').on('click',function () {  
        let id= $(this).data('id');  
        let path= $(this).data('path');  
  
        $("#delete-modal").modal('show');  
        $("#deleteID").val(id);  
        $("#deleteFilePath").val(path)  
    })  
</script>
// script end

```

## 8. Eye Icon in password field

```html 
//password
<div class="mb-3 position-relative">  
    <label for="password" class="form-label">Password<span class="text-danger">*</span></label>  
    <div class="input-group">  
        <input type="password" class="form-control" id="crtPassword" name="password" placeholder="Enter password" required>  
        <button type="button" class="btn toggle-password" style="background-color: white; border: 1px solid #ced4da;border-left: none; box-shadow: none;" tabindex="-1">  
            <i class="bi bi-eye-slash" id="togglePasswordIcon"></i>  
        </button>    </div>    @error('password')  
    <div class="text-danger">{{ $message }}</div>  
    @enderror  
</div>

// confirm password
<div class="mb-3 position-relative">  
    <label for="password_confirmation" class="form-label">Confirm Password<span class="text-danger">*</span></label>  
    <div class="input-group">  
        <input type="password" class="form-control" id="crtPassword_confirmation" name="password_confirmation" placeholder="Enter confirm password" required>  
        <button type="button" class="btn toggle-password" style="background-color: white; border: 1px solid #ced4da;border-left: none; box-shadow: none;" tabindex="-1">  
            <i class="bi bi-eye-slash" id="togglePasswordIcon"></i>  
        </button>    </div>    @error('password_confirmation')  
    <div class="text-danger">{{ $message }}</div>  
    @enderror  
</div>


<style>  
	.toggle-password {  
	background-color: white;  
	border: 1px solid #ced4da; /* Matches the input field border */  
	box-shadow: none; /* Removes shadow */  
	}  

	.toggle-password:hover,  
	.toggle-password:focus {  
	background-color: white; /* Keeps the background white on hover */  
	border-color: #ced4da; /* Ensures the border doesn't change */  
	box-shadow: none; /* Prevents focus shadow */  
	cursor: pointer;  
	}  

	.toggle-password i {  
	color: #6c757d; /* Matches the text color of the input */  
	}  
  
</style>

// eye icon start  
<script>
document.addEventListener("DOMContentLoaded", function () {  
    const togglePasswordButtons = document.querySelectorAll(".toggle-password");  
  
    togglePasswordButtons.forEach(button => {  
        button.addEventListener("click", function () {  
            const input = this.previousElementSibling; // Get the input field  
            const icon = this.querySelector("i"); // Get the icon inside the button  
  
            if (input.type === "password") {  
                input.type = "text"; // Change input type to text  
                icon.classList.remove("bi-eye-slash");  
                icon.classList.add("bi-eye"); // Change icon to eye  
            } else {  
                input.type = "password"; // Change input type to password  
                icon.classList.remove("bi-eye");  
                icon.classList.add("bi-eye-slash"); // Change icon to eye-slash  
            }  
        });  
    });  
});  
  
</script>
// eye icon end
```

## 9. Normal Modal
```html
<div class="align-items-center col">  
    <button data-bs-toggle="modal" data-bs-target="#create-modal" class="float-end btn m-0 bg-gradient-primary">Create</button>  
</div>  
  
<div class="modal animated zoomIn modal-xl smooth-modal" id="create-modal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">  
    <div class="modal-dialog modal-dialog-centered modal-md">  
        <div class="modal-content">  
            <div class="modal-header">  
                <h6 class="modal-title" id="exampleModalLabel">Create Category</h6>  
            </div>            
            <div class="modal-body">  
                <form id="save-form">  
                    <div class="container">  
                        <div class="row">  
                            <div class="col-12 p-1">  
                                <label class="form-label">Category Name *</label>  
                                <input type="text" name="categoryName" class="form-control" id="categoryName">  
                            </div>                        </div>                      
                    </div>                  
                </form>              
            </div>              
			<div class="modal-footer">  
                <button id="modal-close" class="btn bg-gradient-primary" data-bs-dismiss="modal" aria-label="Close">Close</button>  
                <button onclick="Save()" id="save-btn" class="btn bg-gradient-success" >Save</button>  
            </div>        
        </div>      
    </div>  
</div>
```
## 10. Full -> Blade to Controller ( Modal ar maddhome blade file open hobe )
```html 
<button data-bs-toggle="modal" data-bs-target="#create-modal" class="createBrandButton">  
    <i class="bi bi-plus-circle"></i> Create Brand  
</button>
<div class="modal animated zoomIn modal-xl smooth-modal" id="create-modal" tabindex="-1" aria-labelledby="exampleModalLabel1" aria-hidden="true">  
    <div class="modal-dialog modal-dialog-centered modal-md">  
        <div class="modal-content">  
            <div class="modal-header">  
                <h6 class="modal-title" id="exampleModalLabel1">Create Brand</h6>  
            </div>            
            <div class="modal-body">  
                <form id="save-form">  
                    <div class="container">  
                        <div class="row">  
                            <div class="col-12 p-1">  
                                <label class="form-label">Brand Name<span class="text-danger">*</span></label>  
                                <input type="text" name="name" class="form-control" id="name">  
                            </div> 
                            <div class="col-12 p-1">  
                                <label class="form-label">Brand Description<span class="text-danger">*</span></label>  
                                <input type="text" name="description" class="form-control" id="description">  
                            </div>                        
                        </div>                    
                    </div>                
                </form>            
            </div>            
	        <div class="modal-footer">  
	            <button id="modal-close" class="btn commonCloseButton" data-bs-dismiss="modal" aria-label="Close">Close</button>  
	            <button onclick="brandSave()" id="save-btn" class="btn commonSaveButton" >Save</button>  
	        </div>       
        </div>    
    </div>
</div>
```

```javaScript
async function brandSave() { 
try { 
	    let name = document.getElementById('name').value; 
	    let description = document.getElementById('description').value;
	     
	    if (name.trim().length === 0) {  
	        errorToast('Name is required!');  
	        return; // ekhanei stop kore dibe  , ei return ta modal ar jonno dite hoy , normal page hole deya lagto na.
	    }
	    // description jodi nullable rakhte chao tahoel condition deya lagbe na.
	    if(description.trim().length === 0){
	    errorToast('Description is required!');  
	        return; // stop execution  
	    }  
        
        showloader();
        let res = await axios.post("/partner/create-partner-brand-modal", { name: name, description:description });  
		hideloader();
        // successfull create status=201 
        if (res.status === 201) {  
            successToast('Brand created successfully');  
            // Close the modal  
            document.getElementById('modal-close').click();  
            // Reset the form  
            document.getElementById("save-form").reset();
            // created brand k dekhanor jonno  
            await getBrandList();  
        }
    } 
    catch (error) {   
        if (error.response && error.response.status === 422) {  
            // unique kina check korar jonno 
            errorToast('The Brand Name has already been taken'); 
        }
        else { 
	        errorToast('An error occurred. Please try again.'); 
        } 
    }  
}
```

```php
public function createPartnerBrandModal(Request $request)  
{  
    try {  
        $validated = $request->validate([  
            'name' => 'required|string|max:255|unique:brands,name',
            'description' => 'nullable|string|max:255',  
        ]); 
         
        return Brand::create($validated);
    }
    catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors , unique kina check korar jonno  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    }
    catch (\Exception $e) { 
    // Handle other exceptions (e.g., database errors) 
    return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500); }
}



// Alternative 

		// Note:  uporer controller use na kore jodi nicher controller use kori tahole js ar nicher code tuku likha lagebe na.uporer controller use korleo validation hoye jabe kintu customize korte                    nicher ta sundor
		let name = document.getElementById('name').value; 
	    let description = document.getElementById('description').value;
	     
	    if (name.trim().length === 0) {  
	        errorToast('Name is required!');  
	        return; // ekhanei stop kore dibe  , eta modal ar jonno dite hoy , normal page hole deya lagto na.
	    }
	    // description jodi nullable rakhte chao tahoel condition deya lagbe na.
	    if(description.trim().length === 0){
	    errorToast('Description is required!');  
	        return; // stop execution  
	    } 

//  alternative Start
public function createPartnerBrandModal(Request $request)  
{  
    try {  
        $validated = $request->validate([  
            'name' => 'required|string|max:255|unique:brands,name',
            'description' => 'nullable|string|max:255',  
        ],[                                                
	        'name.required' => 'The Brand Name is required!',  
            'name.unique' => 'This Brand Name has already been taken',  
        ]); 
	
        return Brand::create($validated);
    }
    catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors , unique kina check korar jonno  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    }
    catch (\Exception $e) { 
    // Handle other exceptions (e.g., database errors) 
    return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500); }
}
// alternative End

```

## 11 . Normal page (Modal na) ar jonno laravel and js code
```js
// js start
async function brandSave() { 
	try { 
	    let name = document.getElementById('name').value; 
	    let description = document.getElementById('description').value;
	     
	    if (name.trim().length === 0) {  
	        errorToast('Name is required!');  
	    }
	    // description jodi nullable rakhte chao tahoel condition deya lagbe na.
	    else if(description.trim().length === 0){
	    errorToast('Description is required!');  
	    }  
        else{
	        showloader();
	        let res = await axios.post("/partner/create-partner-brand-modal", { name: name, description:description });  
			hideloader();
	        // successfull create status=201 
	        if (res.status === 201) {  
	            successToast('Brand created successfully');  
	            // Close the modal  
	            document.getElementById('modal-close').click();  
	            // Reset the form  
	            document.getElementById("save-form").reset();
	            // created brand k dekhanor jonno  
	            await getBrandList();  
	        }
	    } 
    }
    catch (error) {   
        if (error.response && error.response.status === 422) {  
            // unique kina check korar jonno 
            errorToast('The Brand Name has already been taken'); 
        }
        else { 
	        errorToast('An error occurred. Please try again.'); 
        } 
    }  
}
// js end
// php start
public function createPartnerBrandModal(Request $request)  
{  
    try {  
        $validated = $request->validate([  
            'name' => 'required|string|max:255|unique:brands,name',
            'description' => 'nullable|string|max:255',  
        ]); 
         
        return Brand::create($validated);
    }
    catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors , unique kina check korar jonno  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    }
    catch (\Exception $e) { 
    // Handle other exceptions (e.g., database errors) 
    return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500); }
}
// php end


```

## 12. Different approach of controller validation
```php

// some approach of validation
 $validated = $request->validate([  
	'name' => 'required|string|max:255|unique:brands,name',
	'description' => 'nullable|string|max:255',  
]);
1.
$brand = Brand::create( $validated + [ 
	'logo' => 'brand-logo/default_logo.png', 
	'partner_id' => $partner->id, 
]);
2.
$brand = Brand::create(array_merge($validated, [ 
	'logo' => 'brand-logo/default_logo.png', 
	'partner_id' => $partner->id, 
]));
3.
$brand = Brand::create([
    ...$validated, 
    'logo' => 'brand-logo/default_logo.png',
    'partner_id' => $partner->id,
]);


// another approach

//controller start
public function createPartnerBrandModal(Request $request)  
{  
    try {  
        $validated = $request->validate([  
            'name' => 'required|string|max:255|unique:brands,name',  
        ]);  
  
        $partner = Auth::user()->partner()->first();  
  
        if (!$partner) {  
            return response()->json(['error' => 'Partner not found.'], 404);  
        }  
  
        $brand = Brand::create([  
            'name'       => $validated['name'],  
            'logo' => 'brand-logo/default_logo.png',  
            'partner_id' => $partner->id,  
        ]);  
  
        return response()->json(['message' => 'Brand created successfully.', 'brand' => $brand], 201);  
    } catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    } catch (\Exception $e) {  
        // Handle other exceptions  
        return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500);  
    }  
}


// js code start
async function brandSave() {  
    try {  
  
        let brandName = document.getElementById('brandName').value;  
        if (brandName.trim().length === 0) {  
            errorToast('Brand Name is required!');  
            return;  
        }  
  
        let res = await axios.post("/partner/create-partner-brand-modal", { name: brandName });  
  
        if (res.status === 201) {  
            successToast('Brand created successfully');  
            // Close the modal  
            document.getElementById('modal-close1').click();  
            // Reset the form  
            document.getElementById("save-form-brand").reset();  
  
            await fetchBrands();  
        } else {  
            errorToast('An unexpected error occurred!');  
        }  
    }  
    catch (error) {  
        // Handle validation or server errors  
        if (error.response && error.response.status === 422) {  
            // unique kina check korar jonno  
            let errorMessage = error.response.data.details.name?.[0] || 'Validation error';  
            errorToast(errorMessage);  
			// if (error.response && error.response.status === 422) {  
			//     errorToast('The Brand Name has already been taken');  
        } else if (error.response && error.response.status === 404) {  
            // Partner not found  
            errorToast('Partner not found.');  
        }  
    }  
}
```