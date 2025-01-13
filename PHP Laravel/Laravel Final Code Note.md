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
                            </div>                        
                        </div>                      
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
## ***10. Full using Laravel
```html
// form start
<div class="card">  
        <div class="card-body">  
            <form action="{{ route('partner.team.member.store') }}" method="POST">  
                @csrf  
                <div class="mb-3">  
                    <label for="name" class="form-label">Full Name<span class="text-danger">*</span></label>  
                    <input type="text" class="form-control" id="name" name="name" placeholder="Enter Full Name" value="{{ old('name') }}" required>  
                    @error('name')  
                    <div class="text-danger">{{ $message }}</div>  
                    @enderror  
                </div>  

                <div class="mb-3">  
                    <label for="email" class="form-label">Email Address<span class="text-danger">*</span></label>  
                    <input type="email" class="form-control" id="email" name="email" placeholder="Enter email" value="{{ old('email') }}" required>  
                    @error('email')  
                    <div class="text-danger">{{ $message }}</div>
                    @enderror  
                </div>  

                <div class="mb-3">  
                    <label for="mobile_no" class="form-label">Mobile<span class="text-danger">*</span></label>  
                    <input type="text" class="form-control" id="mobile_no" name="mobile_no" placeholder="01*********" value="{{ old('mobile_no') }}" required>  
                    @error('mobile_no')  
                    <div class="text-danger">{{ $message }}</div>  
                    @enderror  
                </div>  

                <div class="mb-3">  
                    <label for="password" class="form-label">Password<span class="text-danger">*</span></label>  
                    <input type="password" class="form-control" id="password" name="password" placeholder="Enter password" required>  
                    @error('password')  
                    <div class="text-danger">{{ $message }}</div>  
                    @enderror  
                </div>  

                <div class="mb-3">  
                    <label for="password_confirmation" class="form-label">Confirm Password<span class="text-danger">*</span></label>  
                    <input type="password" class="form-control" id="password_confirmation" name="password_confirmation" placeholder="Enter confirm password" required>  
                    @error('password_confirmation')  
                    <div class="text-danger">{{ $message }}</div>  
                    @enderror  
                </div>  

                <input type="hidden" value="team_member" name="client_type" />  
                <button class="btn btn-primary">Add Team Member</button>  
            </form>  
        </div>  
    </div>
// form end
```

```php
// Controller Start
public function store(Request $request) {
	try {
		$validated = $request->validate([
	        'name'  => 'required|string|max:255|unique:users,username',
	        'email'     => 'required|email|unique:users,email',
	        'mobile_no' => 'required|string|unique:users,mobile_no',
	        'password'  => 'required|confirmed|min:8',
	    ], [
		    'name.required' => 'The Full Name is required.',
	        'name.unique' => 'The Full Name has already been taken.',
	        'email.unique' => 'The email address is already registered.',
	        'mobile_no.unique' => 'The mobile number is already registered.',
	    ]);
	    
		User::create($validated + [
		  'password' => Hash::make($validated['password'])
		]);
		return response()->json(['status' => 'success', 'message' => 'User added successfully!']);
    }
    catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors , unique kina check korar jonno  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    }
    catch (\Exception $e) { 
    // Handle other exceptions (e.g., database errors) 
    return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500); 
    }

}
// Controller End


// Notification start : html ar body ar votor ba common js file a rakhte hobe jeno sob jayga theke access kora jay
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
// Notification end

// link gulao add korte hobe for toastr notification
<script src="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/2.1.4/toastr.min.js" integrity="sha512-lbwH47l/tPXJYG9AcFNoJaTMhGvYWhVM9YI43CT+uteTRRaiLCui8snIgyAN8XWgNjNhCqlAUdzZptso6OCoFQ==" crossorigin="anonymous" referrerpolicy="no-referrer"></script>

<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/toastr.js/2.1.4/toastr.min.css" integrity="sha512-6S2HWzVFxruDlZxI3sXOZZ4/eJ8AcxkQH1+JjSe/ONCEqR9L4Ysq5JdT5ipqtzU7WHalNwzwBv+iE51gNHJNqQ==" crossorigin="anonymous" referrerpolicy="no-referrer" />

```
## ***11. Full using Axios 
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
	            <button onclick="Save()" id="save-btn" class="btn commonSaveButton" >Save</button>  
	        </div>       
        </div>    
    </div>
</div>
```

```js
// uniquness check korar jonno evabe try catch korte hoy ta na hole axios kaj hobe na.
async function save() {  
    let name = document.getElementById('name').value;  
    let description = document.getElementById('description').value;  
    
    if (name.trim().length === 0) {  
        errorToast("Name is Required !")  
    } else if (description.trim().length === 0) {  
        errorToast("Description is Required !")     
    } else {  
        try {  
	        showloader();
            let res = await axios.post('/partner/team-member-store', {  
                name: name,  
                description: description,   
            });  
            hideloader();
            if (res.data['status'] === 'success') {  
                document.getElementById('modal-close').click();  
                successToast('Team Member Added successfully')  
                document.getElementById('save-form').reset();  
                window.location.reload(); // await getlist(); etao use kora jay, etar mane holo getList k ekhan hote call kora. 
            } else {  
                errorToast('Team Member Added Failed')  
            }  
        } catch (error) {  
            // response(error); // eta ekta method, ekhan theke function call kora jabe common ekta function create kore 
            if (error.response) {  
                if (error.response.status === 500) {  
                    errorToast("Server Error: Please try again later.");  
                } else if (error.response.status === 422) {  // 422 is the status code for validation errors  
                    let errors = error.response.data.messages;  
                    for (let key in errors) {  
                        if (errors.hasOwnProperty(key)) {  
                            errors[key].forEach(message => {  
                                errorToast(message);  
                            });  
                        }  
                    }  
                } else {  
                    errorToast("Error: " + error.response.data.message || "Something went wrong!");  
                }  
            } else {  
                errorToast(error.response.data.message);  
            }  
        }  
    }  
}
```

```php
public function partnerStore(Request $request)  
{  
	try {
	    $request->validate([  
	        'name' => 'required|string|max:255|unique:clients,name',  
	        'description' => 'required|string|max:255',    
	        ], [  
	            'name.unique' => 'The name has already been taken.',  
	            'description.required' => 'The description is required.',    
	    ]);  
	    User::create($validated);
		return response()->json(['status' => 'success', 'message' => 'User added successfully!']);
    }
    catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors , unique kina check korar jonno  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    }
    catch (\Exception $e) { 
    // Handle other exceptions (e.g., database errors) 
    return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500); 
    }


}
```

```js
// Notification code start
// Add link in the head of html template and Download this link,script from website
<link href="{{asset('css/toastify.min.css')}}" rel="stylesheet" />
<script src="{{asset('js/jquery-3.7.0.min.js')}}"></script>
<script src="{{asset('js/toastify-js.js')}}"></script>
//  axios use korte chaile may be cdn or axios js code download kore use kora lage jene nibe not sure 


//Add this code in config.js ( Success and Error Toast  JavaScript code )
function showLoader() {  
    document.getElementById('loader').classList.remove('d-none')  
}  
function hideLoader() {  
    document.getElementById('loader').classList.add('d-none')  
}  
  
function successToast(msg) {  
    Toastify({  
        text: msg,  
        gravity: "bottom", // `top` or `bottom`  
        position: "center", // `left`, `center` or `right`  
        className: "mb-5",  
        duration: 3000,  
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
        duration: 3000,  
        style: {  
            background: "red", // style er vetore aro onk kichu add kora jabe 
        }  
    }).showToast();  
}


// Alternative code for notification
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
// notification end
```
## ***12. Full using Axios ( Modal ar upor abar modal use korle )
```html 
// button modal-1
<div class="col-md-6 text-end">  
    <button class="commonButton" data-bs-toggle="modal" data-bs-target="#add-team-member" ><i class="bi bi-plus-circle me-2"></i>Add Team Member</button>  
</div>
// modal-1 start
<div class="modal modal-lg animated zoomIn" id="add-team-member" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">  
    <div class="modal-dialog modal-dialog-centered modal-md">  
        <div class="modal-content">  
            <div class="modal-header">  
                <h6 class="modal-title" id="exampleModalLabel" style="color: #2f322c">Add Team Member</h6>  
            </div>            
            <div class="modal-body">  
                <form id="save-form">  
		            // button modal-2                         
                    <button data-bs-toggle="modal" data-bs-target="#create-modal1" class="createBrandButton">  
                        <i class="bi bi-plus-circle"></i>  
                    </button>                                               
                    <div class="mb-3">  
                        <label for="username" class="form-label">Full Name<span class="text-danger">*</span></label>  
                        <input type="text" class="form-control" id="crtUsername" name="username" placeholder="Enter Full Name" value="{{ old('username') }}" required>  
                        @error('username')  
                          <div class="text-danger">{{ $message }}</div>  
                        @enderror  
                    </div>                 
                </form>  
            </div>            
            <div class="modal-footer">  
                <button type="button" class="commonCloseButton" id="modal-close" data-bs-dismiss="modal" aria-label="Close">Close</button>  
                <button onclick="save()" class="commonSaveButton">Save</button>  
            </div>        
        </div>    
    </div>
</div>

// modal-1 end

{{-- modal-2 start--}}  
	<div class="modal animated zoomIn" id="create-modal1" tabindex="-1" aria-labelledby="exampleModalLabel1" aria-hidden="true">  
		<div class="modal-dialog modal-dialog-centered modal-md">  
			<div class="modal-content">  
				<div class="modal-header">  
					<h6 class="modal-title" id="exampleModalLabel1">Create Brand</h6>  
				</div>                    
				<div class="modal-body">  
					<form id="save-form-brand">  
						<div class="container">  
							<div class="row">  
								<div class="col-12 p-1">  
									<label class="form-label">Brand Name<span class="text-danger">*</span></label>  
									<input type="text" name="brandName" class="form-control" id="brandName">  
								</div>                                
							</div>                            
						</div>                        
					</form>                    
				</div>                    
				<div class="modal-footer">  
					<button id="modal-close1" class="btn commonCloseButton" data-bs-dismiss="modal" aria-label="Close">Close</button>  
					<button onclick="brandSave()" id="save-btn" class="btn commonSaveButton" >Save</button>  
				</div>                
			</div>            
		</div>       
	</div>
{{--modal-2 end--}}

```

```javaScript
async function Save() { 
	
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
	try { 
		showloader();
		let res = await axios.post("/partner/create-partner-brand-modal", { name: name, description:description });  
		hideloader();
		// successfull create status=201 
		if (res.data['status'] === 'success') {  
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
		// response(error); // eta ekta method, ekhan theke function call kora jabe common ekta function create kore 
		if (error.response) {  
			if (error.response.status === 500) {  
				errorToast("Server Error: Please try again later.");  
			} else if (error.response.status === 422) {  // 422 is the status code for validation errors  
				let errors = error.response.data.messages;  
				for (let key in errors) {  
					if (errors.hasOwnProperty(key)) {  
						errors[key].forEach(message => {  
							errorToast(message);  
						});  
					}  
				}  
			} else {  
				errorToast("Error: " + error.response.data.message || "Something went wrong!");  
			}  
		} else {  
			errorToast(error.response.data.message);  
		}  
	}  
}
```

```php
// php start
public function createBrand(Request $request) {  
   try{
		$validated = $request->validate([  
			'name' => 'required|string|max:255|unique:brands,name',
			'description' => 'nullable|string|max:255',  
		]); 
		 
		Brand::create($validated);
		return response()->json(['status' => 'success', 'message' => 'Brand created successfully']);
    }
    catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors , unique kina check korar jonno  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    }
    catch (\Exception $e) { 
    // Handle other exceptions (e.g., database errors) 
    return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500); 
    }
}
// php end
```
## ***13. Full using Axios ( Card use kore, single input field ar jonno code )
```html
//html start
	<div class="card">  
        <div class="card-body">  
            <form action="" method="">    
                <div class="mb-3">  
                    <label for="name" class="form-label">Brand Name<span class="text-danger">*</span></label>  
                    <input type="text" class="form-control" id="name" name="name" placeholder="Enter Full Name" value="{{ old('name') }}" required>  
                </div>   
                <button onclick="Save()" type="" class="btn btn-primary">Add Team Member</button>  
            </form>  
        </div>  
    </div>
// html end
```

```js
// js start
async function Save() { 
	
	let name = document.getElementById('brand_name').value; 
	
	if (name.trim().length === 0) {  
		errorToast('Brand Name is required!');  
	}
	else{
	    try { 
		        showloader();
		        let res = await axios.post("/partner/create-partner-brand-modal", { name: name });  
				hideloader();
		        // successfull create status=201 
		        if (res.data['status'] === 'success') {  
		            successToast('Brand created successfully');  
		            // Close the modal  
		            document.getElementById('modal-close').click();  
		            // Reset the form  
		            document.getElementById("save-form").reset();
		            // created brand k dekhanor jonno  
		            await getBrandList();  
		        }
		    } 
	    catch (error) {   // single input field ar jonno , multiple field hole error dekhanor jonno may be loop use korte hobe. 
	        if (error.response && error.response.status === 422) {  
	            // unique kina check korar jonno 
	            errorToast('The Brand Name has already been taken'); 
	        }
	        else { 
		        errorToast('An error occurred. Please try again.'); 
	        } 
	    }  
    }
}
// js end
```

```php 
// php start
public function createBrand(Request $request)  
{  
    try {  
        $validated = $request->validate([  
            'name' => 'required|string|max:255|unique:brands,name',
        ]); 
        
        Brand::create($validated);
        return response()->json(['status' => 'success', 'message' => 'Brand created successfully']);
    }
    catch (\Illuminate\Validation\ValidationException $e) {  
        // Handle validation errors , unique kina check korar jonno  
        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
    }
    catch (\Exception $e) { 
    // Handle other exceptions (e.g., database errors) 
    return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500); 
    }
}




// Alternative approach just for understanding
// js a nicher code likhar dorkar nai jodi controller theke validation kori
let name = document.getElementById('name').value; 
let email = document.getElementById('email').value;

if (name.trim().length === 0) {  
	errorToast('Name is required!');  
}
// description jodi nullable rakhte chao tahoel condition deya lagbe na.
else if(email.trim().length === 0){
errorToast('Email is required!');

//  alternative php Start
public function createBrand(Request $request) {  
    try {  
        $validated = $request->validate([  
            'name' => 'required|string|max:255|unique:brands,name',
            'email' => 'nullable|email|max:255',  
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
// alternative php End

// php end
```
## 14. Different approach of controller validation
```php
// some approach of validation start
 $validated = $request->validate([  
	'name' => 'required|string|max:255|unique:brands,name',
	'description' => 'nullable|string|max:255',  
]);

1. $brand = Brand::create( $validated + [ 
	'logo' => 'brand-logo/default_logo.png', 
	'partner_id' => $partner->id, 
   ]);
2. $brand = Brand::create(array_merge($validated, [ 
	'logo' => 'brand-logo/default_logo.png', 
	'partner_id' => $partner->id, 
	]));
3. $brand = Brand::create([
    ...$validated, 
    'logo' => 'brand-logo/default_logo.png',
    'partner_id' => $partner->id,
   ]);
4. $brand = Brand::create([  
    'name'       => $validated['name'],  
    'logo' => 'brand-logo/default_logo.png',  
    'partner_id' => $partner->id,  
	]);
5. $validated = $request->validate([  
    'name' => 'required|string|max:255|unique:brands,name',
    'description' => 'nullable|string|max:255',  
    ]);         
$brand = Brand::create($validated);
6. User::create( $validated + [ 
	'password' => bcrypt($validated['password']) 
	]); 
7.
// alternative start
	User::create([ 
	'name' => $request->name, 
	'email' => $request->email, 
	'mobile_no' => $request->mobile_no, 
	'password' => Hash::make($request->password) 
	]);
	// alternative
	User::create([
		'name' => $request->name,
		'email' => $request->email,
		'mobile_no' => $request->mobile_no,
		'password' => bcrypt($request->password),
	]);
	// alternative
	User::create( $validated + [ 
	'password' => bcrypt($validated['password']) 
	]);
// alternative end

// some approach of validation end

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
## 15. hover korle modal open hobe
```html
<button id="hover-btn" class="btn m-0 badge bg-secondary">How does it work?</button>

<div class="modal modal-xl animated zoomIn smooth-modal" id="create-modal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">  
                <div class="modal-dialog modal-dialog-centered modal-md">  
                    <div class="modal-content">  
                        <div class="modal-header">  
                            <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>  
                        </div>                        <div class="modal-body">  
                            <form id="save-form">  
                                <div class="container">  
                                    <div class="row">  
                                        <div class="col-12 p-1">  
                                            <h4>Website Objective: <span style="color:#2f322c">Premium Ad</span></h4>  
                                                <p>Premium Ad is a comprehensive service platform designed to empower individuals and businesses in creating high-quality advertisements. Below is a structured description of the website's objective:</p>  
                                            <h4>Key Features and Offerings:</h4>  
                                            <h5 class="premiumAdDes">1. Ad Content Creation:</h5>  
                                                <ul class="premiumAdDes">  
                                                    <li>Create custom posts tailored for various advertising needs.</li>  
                                                    <li>Generate photo advertisements with professional-grade tools.</li>  
                                                    <li>Develop engaging ad texts to capture audience attention.</li>  
                                                    <li>Produce high-quality ad videos for impactful marketing.</li>  
                                                </ul>                                           <h5 class="premiumAdDes">2. Brand Creation:</h5>  
                                                <ul class="premiumAdDes">  
                                                    <li>Primary Requirement: Users must create a brand to access the platform's features.</li>  
                                                    <li>The brand serves as the foundation for organizing all advertising activities, ensuring consistency and professionalism.</li>  
                                                    <li>Without creating a brand, users cannot proceed to create posts, photos, videos, or other ad content.</li>  
                                                </ul>                                           <h5 class="premiumAdDes"> 3. User Permissions:</h5>  
                                                <ul class="premiumAdDes">  
                                                    <li>If a user has not yet created a brand, they will be prompted to do so.</li>  
                                                    <li>A dedicated "Create Brand" button allows users to seamlessly start their branding process.</li>  
                                                </ul>                                            <h5 class="premiumAdDes">4. Seamless Workflow:</h5>  
                                                <ul class="premiumAdDes">  
                                                    <li>After creating a brand, users gain access to all ad creation tools.</li>  
                                                    <li>An intuitive interface ensures easy navigation and efficient workflow management.</li>  
                                                </ul>                                            <h5 class="premiumAdDes">5. Purpose:</h5>  
                                                <ul class="premiumAdDes">  
                                                    <li>To simplify the advertisement creation process for businesses and individuals.</li>  
                                                    <li>To help users establish their brand identity before diving into marketing campaigns.</li>  
                                                    <li>To provide all essential tools in one platform for consistent and high-quality ad production.</li>  
                                                </ul>                                            <h5 class="premiumAdDes">6. Target Audience:</h5>  
                                                <ul class="premiumAdDes">  
                                                    <li>Small, Medium and Big-sized businesses seeking professional ad creation tools.</li>  
                                                    <li>Content creators and marketers aiming to enhance their advertising impact.</li>  
                                                    <li>Individuals who need a simple yet powerful platform to create branded advertisements.</li>  
                                                </ul>                                        </div>                                    </div>                                </div>                            </form>                        </div>                        <div class="modal-footer">  
{{--                            <button id="modal-close" class="btn bg-gradient-primary" data-bs-dismiss="modal" aria-label="Close">Close</button>--}}  
                        </div>  
                    </div>                </div>            </div>


```
```js
<script>  
    document.addEventListener("DOMContentLoaded", () => {  
        const hoverBtn = document.getElementById("hover-btn");  
        const createModalEl = document.getElementById("create-modal");  
        const createModal = new bootstrap.Modal(createModalEl);  
  
        // Avoid redundant modal triggers  
        let isModalOpen = false;  
  
        // Show the modal on hover  
        hoverBtn.addEventListener("mouseover", () => {  
            if (!isModalOpen) {  
                createModal.show();  
                isModalOpen = true;  
            }  
        });  
  
        // Detect when the modal is closed to reset state  
        createModalEl.addEventListener("hidden.bs.modal", () => {  
            isModalOpen = false;  
        });  
    });  
</script>  
  
<style>  
    .modal-backdrop {  
        transition: opacity 0.3s ease;  
        background-color: rgba(0, 0, 0, .5); /* Darker backdrop */  
    }  
  
    .modal-backdrop.show {  
        opacity: 0.8; /* Higher opacity for the backdrop when modal is open */  
    }  
  
    /* Modal content transition */  
    .modal-content {  
        transition: transform 0.3s ease, opacity 0.3s ease;  
    }  
  
    .modal.fade .modal-dialog {  
        transform: translateY(-10%);  
        opacity: 0;  
    }  
  
    .modal.show .modal-dialog {  
        transform: translateY(0);  
        opacity: 1;  
    }  
</style>
```
## 16.  Showloader and Hideloader code
```js 
//body ar vetor thake

<div id="loader" class="LoadingOverlay d-none">  
    <div class="Line-Progress">  
        <div class="indeterminate"></div>  
    </div>
</div>

// common js ar vetor thake

function showLoader() {  
    document.getElementById('loader').classList.remove('d-none')  
}  
function hideLoader() {  
    document.getElementById('loader').classList.add('d-none')  
}
```
