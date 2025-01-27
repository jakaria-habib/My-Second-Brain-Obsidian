## ***1. Full Laravel Code with request validation alada rakhbe, Image gulo Helper method a rakhbe  
```html
// Form start
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
// Form end
```
```php
// Controller Start
// Create or store Controller Code Start
 public function store(StoreUserRequest $request) {
	try {	
		$validated = $request->validated();
		if($request->hasFile('image')){
			$imageUrl = ImageHelper::uploadImage('uploads', $request->file('image'));
			$validated['image'] = $imageUrl;
		}
		$validated['password'] = Hash::make($validated['password']);
		
		User::create($validated);
		return response()->json(['status' => 'success', 'message' => 'User created successfully' ]);
		}
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
// Create or store Controller Code End

// Update Controller code Start
public function update(StoreUserRequest $request, $id) {
    $user = User::findOrFail($id); 
    $validated = $request->validated();

    if ($request->hasFile('image')) {
        if ($user->image) {
            ImageHelper::deleteImage($user->image);
        }
        $imageUrl = ImageHelper::uploadImage('uploads', $request->file('image'));
        $validated['image'] = $imageUrl;
    }
	// uporer line tar short kore evabeo likha jay
	// if ($request->hasFile('image')) {
	//    if ($user->image) ImageHelper::deleteImage($user->image);
	//    $validated['image'] = ImageHelper::uploadImage('uploads', $request->file('image'));
	// }
    if (!empty($validated['password'])) {
        $validated['password'] = Hash::make($validated['password']);
    } else {
        unset($validated['password']);
    }
    $user->update($validated);
    return response()->json(['status' => 'success', 'message' => 'User updated successfully']);
}
// Update Controller code End

// nicher command diye request create kore then StoreUserRequest create kore then etar vitore nicher code gulo likhbe
php artisan make:request StoreUserRequest

public function authorize() {
	return true;
}

// StoreUserRequest ar code 
public function rules() {
    $id = $this->route('id') ?? $this->input('id');
    return [
        'name' => 'required|string|max:255|unique:users,name,' . ($Id ?? 'NULL'),
        'age' => 'nullable|integer|min:0|max:200',
        'email' => 'required|email|unique:users,email,' . ($Id ?? 'NULL'),
        'image' => 'nullable|image|mimes:jpg,jpeg,png,gif|max:2048',
        'password' => [$this->isMethod('post') ? 'required' : 'nullable', 'string', 'min:8', 'confirmed'],
    ];
}
public function messages() { 
	return [ 
		'name.required' => 'The name field is required.',
		'name.unique' => 'This name is already taken. Please choose another one.',
		'email.required' => 'The email field is required.',
		'email.unique' => 'This email is already registered. Please use a different email.',
		'password.required' => 'The password field is required.',
		'password.confirmed' => 'The password confirmation does not match.',
		'image.mimes' => 'The image must be a file of type: jpg, jpeg, png, gif.',
		'image.max' => 'The image size should not exceed 2MB.',
		]; 
}

// Image Helper a rakho ei part tuku. App/Helpers/ImageHelper er vetore ImageHelper name ar akta class create kore nite hobe prothome
public static function uploadImage($folder, $file){
	$fileName = uniqid('img-',true) . '.' . $file->getClientOriginalExtension();
	$uploaded = $file->move(public_path($folder), $fileName);
	return $uploaded ? 'uploads/' . $fileName : null;
}	

// Delete Image Helper code 
public static function deleteImage($path) {
    $fullPath = public_path($path);
    if (file_exists($fullPath)) {
        unlink($fullPath);
        return true;
    }
    return false;
}

// Controller End
```
```php
// Laravel Notification start : html ar body ar vetor ba common js file a rakhte hobe jeno sob jayga theke access kora jay
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
## ***2. Controller ar vetorei request validation and image soho sob rekhe kora, uporer  code ta best, follow uporer code
```php
// Store code laravel
public function store(Request $request) {
	try{
	    $validated = $request->validate([
	        'name' => 'required|string|max:255|unique:users,name',
	        'email' => 'required|email|unique:users,email',
	        'image' => 'nullable|image|mimes:jpg,jpeg,png,gif|max:2048',
	        'password' => 'required|string|min:8|confirmed',
	    ], [
	        'name.required' => 'The name field is required.',
	        'name.unique' => 'This name is already taken. Please choose another one.',
	        'email.required' => 'The email field is required.',
	        'email.unique' => 'This email is already registered. Please use a different email.',
	        'password.required' => 'The password field is required.',
	        'password.confirmed' => 'The password confirmation does not match.',
	        'image.mimes' => 'The image must be a file of type: jpg, jpeg, png, gif.',
	        'image.max' => 'The image size should not exceed 2MB.',
	    ]);
	    if ($request->hasFile('image')) {
	        $file = $request->file('image');
	        $imageUrl = 'uploads/' . uniqid('img-', true) . '.' . $file->getClientOriginalExtension();
	        $file->move(public_path('uploads'), $imageUrl);
	        $validated['image'] = $imageUrl;
	    }
	    $validated['password'] = Hash::make($validated['password']);
	    
	    $user = User::create($validated);
	    
	    return response()->json(['status' => 'success', 'message' => 'User created successfully', 'user' => $user]);
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

// Update code laravel
 public function update(Request $request) {
	try{	  
	   $validated = $request->validate([
			'name' => 'required|string|max:255|unique:users,name',
			'email' => 'required|email|unique:users,email',
			'image' => 'nullable|image|mimes:jpg,jpeg,png,gif|max:2048',
			'password' => 'required|string|min:8|confirmed',
		], [
			'name.required' => 'The name field is required.',
			'name.unique' => 'This name is already taken. Please choose another one.',
			'email.required' => 'The email field is required.',
			'email.unique' => 'This email is already registered. Please use a different email.',
			'password.required' => 'The password field is required.',
			'password.confirmed' => 'The password confirmation does not match.',
			'image.mimes' => 'The image must be a file of type: jpg, jpeg, png, gif.',
			'image.max' => 'The image size should not exceed 2MB.',
		]);
		if ($request->hasFile('image')) {
		    if ($user->image && file_exists(public_path($user->image))) {
		        unlink(public_path($user->image));
		    }
		    $file = $request->file('image');
		    $imageUrl = 'uploads/' . uniqid('img-', true) . '.' . $file->getClientOriginalExtension();
		    $file->move(public_path('uploads'), $imageUrl);
		    $validated['image'] = $imageUrl;
		}
		$validated['password'] = Hash::make($validated['password']);
		
		$user = User::create($validated);
		
		return response()->json(['status' => 'success', 'message' => 'User created successfully','user' = $user ]);
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
## 3. ***Full Axios Code with request validation alada rakhbe, Image gulo Helper method a rakhbe 
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
// Controller Start
// Create or store Controller Code Start
 public function store(StoreUserRequest $request) {
	try {	
		$validated = $request->validated();
		if($request->hasFile('image')){
			$imageUrl = ImageHelper::uploadImage('uploads', $request->file('image'));
			$validated['image'] = $imageUrl;
		}
		$validated['password'] = Hash::make($validated['password']);
		
		User::create($validated);
		return response()->json(['status' => 'success', 'message' => 'User created successfully' ]);
		}
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
// Create or store Controller Code End

// Update Controller code Start
public function update(StoreUserRequest $request, $id) {
    $user = User::findOrFail($id); 
    $validated = $request->validated();

    if ($request->hasFile('image')) {
        if ($user->image) {
            ImageHelper::deleteImage($user->image);
        }
        $imageUrl = ImageHelper::uploadImage('uploads', $request->file('image'));
        $validated['image'] = $imageUrl;
    }
	// uporer line tar short kore evabeo likha jay
	// if ($request->hasFile('image')) {
	//    if ($user->image) ImageHelper::deleteImage($user->image);
	//    $validated['image'] = ImageHelper::uploadImage('uploads', $request->file('image'));
	// }
    if (!empty($validated['password'])) {
        $validated['password'] = Hash::make($validated['password']);
    } else {
        unset($validated['password']);
    }
    $user->update($validated);
    return response()->json(['status' => 'success', 'message' => 'User updated successfully']);
}
// Update Controller code End

// nicher command diye request create kore then StoreUserRequest create kore then etar vitore nicher code gulo likhbe
php artisan make:request StoreUserRequest

public function authorize() {
	return true;
}

// StoreUserRequest ar code 
public function rules() {
    $id = $this->route('id') ?? $this->input('id');
    return [
        'name' => 'required|string|max:255|unique:users,name,' . ($Id ?? 'NULL'),
        'age' => 'nullable|integer|min:0|max:200',
        'email' => 'required|email|unique:users,email,' . ($Id ?? 'NULL'),
        'image' => 'nullable|image|mimes:jpg,jpeg,png,gif|max:2048',
        'password' => [$this->isMethod('post') ? 'required' : 'nullable', 'string', 'min:8', 'confirmed'],
    ];
}
public function messages() { 
	return [ 
		'name.required' => 'The name field is required.',
		'name.unique' => 'This name is already taken. Please choose another one.',
		'email.required' => 'The email field is required.',
		'email.unique' => 'This email is already registered. Please use a different email.',
		'password.required' => 'The password field is required.',
		'password.confirmed' => 'The password confirmation does not match.',
		'image.mimes' => 'The image must be a file of type: jpg, jpeg, png, gif.',
		'image.max' => 'The image size should not exceed 2MB.',
		]; 
}

// Image Helper a rakho ei part tuku. App/Helpers/ImageHelper er vetore ImageHelper name ar akta class create kore nite hobe prothome
public static function uploadImage($folder, $file){
	$fileName = uniqid('img-',true) . '.' . $file->getClientOriginalExtension();
	$uploaded = $file->move(public_path($folder), $fileName);
	return $uploaded ? 'uploads/' . $fileName : null;
}	

// Delete Image Helper code 
public static function deleteImage($path) {
    $fullPath = public_path($path);
    if (file_exists($fullPath)) {
        unlink($fullPath);
        return true;
    }
    return false;
}
// Controller End
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

or
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
## 4. Java Script and Laravel Notification Code
```html
// Start JavaScript Notification Code

// 1.Add link in the head of html template and Download this link and script from website
<link href="{{asset('css/toastify.min.css')}}" rel="stylesheet" />
<script src="{{asset('js/jquery-3.7.0.min.js')}}"></script>
<script src="{{asset('js/toastify-js.js')}}"></script>
```
```js code
// 2.Add this code in config.js ( Success and Error Toast  JavaScript code )
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
```JavaScript
// 3. Use code in the script tag , this is just for example
let categoryName = document.getElementById('categoryName').value;  
let categoryPrice = document.getElementById('categoryPrice').value;

if (categoryName.length === 0) {  
    errorToast("Category Name is Required !")  
}else if(categoryPrice.length === 0){
	errorToast("Category Price is Required !")  
}else{
	write others code here
}
 // End JavaScript Notification Code
```
```html
// Start Laravel Notification Code
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

// blade ar code amon hote hobe
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
```
```php
// controller code ar return amon hote hobe
return response()->json(['status' => 'success', 'message' => 'User added successfully!']);
```
## 5. JavaScript Show Loader and Hide Loader Code
```JavaScript
// 1. Add this in view blade file  
// just for example
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
            }  
            else{  
                errorToast("Request fail !")  
            }  
        }  
    }  
</script>
```
```JavaScript
// 2  Add this in config.js file
//body ar vetor likhte hobe
<div id="loader" class="LoadingOverlay d-none">  
    <div class="Line-Progress">  
        <div class="indeterminate"></div>  
    </div>
</div>

// common js ar vetor likhte hobe
function showLoader() {  
    document.getElementById('loader').classList.remove('d-none')  
}  
function hideLoader() {  
    document.getElementById('loader').classList.add('d-none')  
}
```

## 6. Validation Code  Example

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

## 7. Dropdown list using select option with Select2 with Image

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

## 8. Normal Modal and Delete Modal form  with Bootstrap
 ```html
 
// Normal Modal start
// link start
<link href="{{asset('css/bootstrap.css')}}" rel="stylesheet" />  
<link href="{{asset('css/animate.min.css')}}" rel="stylesheet" />
<script src="{{asset('js/bootstrap.bundle.js')}}"></script>

// html code start
<button data-bs-toggle="modal" data-bs-target="#create-modal" class="float-end btn m-0  bg-gradient-primary">Create</button>  

// fade dile slowly open hoy
<div class="modal animated zoomIn modal-xl smooth-madal fade" id="create-modal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">
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
// Normal Modal End


// Delete Modal Start
// link start
<link href="{{asset('css/bootstrap.css')}}" rel="stylesheet" />  
<link href="{{asset('css/animate.min.css')}}" rel="stylesheet" />
<link href="{{asset('css/style.css')}}" rel="stylesheet" />

<script src="{{asset('js/jquery-3.7.0.min.js')}}"></script>
<script src="{{asset('js/axios.min.js')}}"></script>
<script src="{{asset('js/bootstrap.bundle.js')}}"></script>
// link End

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

// Delete Modal End
```
## 9. Eye Icon in password field

```html 
//password
<div class="mb-3 position-relative">  
    <label for="password" class="form-label">Password<span class="text-danger">*</span></label>  
    <div class="input-group">  
        <input type="password" class="form-control" id="crtPassword" name="password" placeholder="Enter password" required>  
        <button type="button" class="btn toggle-password" style="background-color: white; border: 1px solid #ced4da;border-left: none; box-shadow: none;" tabindex="-1">  
            <i class="bi bi-eye-slash" id="togglePasswordIcon"></i>  
        </button>    
    </div>    
    @error('password')  
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
        </button>    
    </div>    
    @error('password_confirmation')  
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

// eye icon js code start  
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
// eye icon js code end
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
	'password' => bcrypt($validated['password']),
	 
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
## 15. Hover korle modal open hobe
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
                                                <p>Premium Ad is a comprehensive service platform designed to empower. Below is a structured description of the website's objective:</p>  
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
## 17. Button a click korle Modal show korbe with image code select2 diye kora
```html
   {{--    upload modal start--}}  
    <div class="modal animated zoomIn" id="upload-image-modal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">  
        <div class="modal-dialog modal-dialog-centered modal-md">  
            <div class="modal-content">  
                <div class="modal-header">  
                    <h6 class="modal-title" id="exampleModalLabel">Upload Image</h6>  
                </div>                <div class="modal-body">  
                    <form action="{{ route('library.image.upload') }}" method="POST" enctype="multipart/form-data" id="save-form">  
                        <div class="container">  
                            <div class="row">  
  
                                <div class="col-12 p-1">  
                                    <label class="form-label" style="margin-bottom: 1.25rem!important;">Select Brand<span class="text-danger">*</span></label>  
                                    <select name="upload_brand_name" id="upload_brand_name" class="form-select brandSelect w-100 brand_name" required>  
  
                                    </select>                                </div>                                <div class="col-12 p-1">  
                                    <label class="form-label">Select Image<span class="text-danger">*</span></label>  
                                    <input type="file" name="uploadImage" class="form-control uploadImage1" id="uploadLibraryImage">  
                                </div>                            </div>                        </div>                    </form>                </div>                <div class="modal-footer">  
                    <button id="upload-modal-close" onclick="uploadModalClose()" class="commonCloseButton">Close</button>  
                    <button onclick="uploadLibraryImage()" id="save-btn" class="commonSaveButton" >Save</button>  
                </div>            </div>        </div>    </div>{{--    upload modal end--}}
```

```js
// upload image start  
  
// brand fetch start  
async function fetchUploadBrand() {  
    try {  
        const res = await axios.get('/partner/brand-list'); // API call to fetch brand data  
        const brandSelect = $('#upload_brand_name'); // jQuery selector for the dropdown  
  
        // Clear existing options and add a placeholder        brandSelect.empty();  
        brandSelect.append('<option value="">&nbsp;&nbsp;&nbsp;&nbsp;All Assets</option>');  
  
        // Populate the dropdown with options  
        res.data.forEach((brand) => {  
            const option = new Option(brand.name, brand.id, false, false);  
            $(option).attr('data-image', '/'+brand.logo); // Add the custom data attribute  
            brandSelect.append(option); // Append the option to the select element  
        });  
  
        // Trigger change event to ensure Select2 updates  
        brandSelect.trigger('change');  
    } catch (error) {  
        console.error('Error fetching brands:', error); // Handle errors gracefully  
    }  
}  
// fetch end  
  
function initializeSelect22() {  
    $('#upload_brand_name').select2({  
        templateResult: formatOptions,  
        templateSelection: formatOption,  
        // placeholder: 'All Brands List', // Placeholder for Select2  
        // allowClear: true, // Allow clearing the selection        dropdownParent: $('#upload-image-modal'), // Attach dropdown to the modal to avoid z-index issues  
    });  
  
    const customCSS = `.select2-selection__placeholder {  
                            padding-left: 16px;                        }`;  
    $('<style>').text(customCSS).appendTo('head');  
}  
  
// Custom formatting for Select2 options  
function formatOptions(option) {  
    if (!option.id) {  
        return option.text;  
    }  
    const imgUrl = $(option.element).data('image');  
    return $(  
        `<span class="custom-option">  
        <img src="${imgUrl}" alt="icon" style="width: 20px; height: 20px; margin-right: 8px;" />  
        ${option.text}  
    </span>`  
    );  
}  
  
function uploadBrandImage(){  
    document.getElementById('upload-image-modal').style.display = 'block';  
    fetchUploadBrand().then(() => {  
        initializeSelect22();  
    });  
}  
function uploadModalClose(){  
    document.getElementById('upload-image-modal').style.display = 'none';  
}  
  
  
// upload image end
```

## 18. search
```js
@extends('partner.layouts.app')  
@section('page')  
    <!-- Header -->  
    <div class="row align-items-center brandDown">  
        <div class="col-12 col-md-9">  
            <!-- Left Side: Brand List -->  
            <h2 class="brandList">  
                <i class="bi bi-star-fill"></i> Brand List  
                <span class="text-muted">({{ sizeof($brands) }}/10)</span>  
            </h2>            <p>See your projects and create new ones under the selected brand.</p>  
        </div>        <div class="col-12 col-md-3 text-md-end text-center">  
            <select name="brand_name" id="brand_name" class="form-select" style="padding: 8px; margin-bottom: 11px;">  
                <option value="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;All Brands List</option>  
                @foreach ($brands as $brand)  
                    <option value="{{ $brand->id }}" data-name="{{ strtolower($brand->name) }}" data-image="{{ asset($brand->logo) }}">  
                        {{ $brand->name }}  
                    </option>  
                @endforeach  
            </select>  
        </div>    </div>  
    {{-- Cards Grid --}}  
    <div class="row" id="cardContainer">  
        <!-- Create a Brand Card -->  
        <div class="col-md-3 mb-4">  
            <a href="{{ URL::to('/') }}/partner/create-brand" class="card-create" style="text-decoration: none; display: block;">  
                <div class="brand-card">  
                    <i class="bi bi-plus-circle"></i>  
                    <h5 class="createh5">Create a Brand</h5>  
                    <p>Click here to add an additional brand that you can use to generate assets.</p>  
                </div>            </a>        </div>  
        <!-- Brand Cards -->  
        @foreach ($brands as $brand)  
            <div class="col-md-3 mb-4 brand-card-wrapper" data-name="{{ strtolower($brand->name) }}" data-id="{{ $brand->id }}">  
                <a href="{{ URL::to('/') }}/partner/edit-partner-brand/{{ $brand->id }}" class="card-create" style="text-decoration: none; display: block;">  
                    <div class="brand-card text-center">  
                        <img src="{{ asset($brand->logo) }}" alt="Brand Logo" class="mb-2">  
                        <h5>{{ $brand->name }}</h5>  
                        <p>1 Project Created</p>  
                    </div>                </a>            </div>        @endforeach  
  
    </div>  
  
  
  
    <script>  
        $(document).ready(function () {  
            // Initialize Select2 with custom templates  
            $('#brand_name').select2({  
                // placeholder: "All Brands List",  
                // allowClear: true,                templateResult: formatOption,  
                templateSelection: formatOption,  
            });  
  
            function formatOption(option) {  
                if (!option.id) {  
                    return option.text;  
                }  
                const imgUrl = $(option.element).data('image');  
                return $(`<span class="custom-option">  
                        <img src="${imgUrl}" alt="icon" style="width: 20px; height: 20px; margin-right: 5px;">  
                        ${option.text}  
                      </span>`);  
            }  
  
            // Filter cards when an option is selected  
            $('#brand_name').on('change', function () {  
                const selectedBrandId = $(this).val(); // Get selected brand ID  
                filterCardsByBrand(selectedBrandId);  
            });  
  
            // Function to filter cards based on brand ID  
            function filterCardsByBrand(selectedBrandId) {  
                $('.brand-card-wrapper').each(function () {  
                    const cardBrandId = $(this).data('id'); // Get card's brand ID  
                    if (!selectedBrandId || cardBrandId == selectedBrandId) {  
                        $(this).show(); // Show card if it matches the selected ID or "All Brands List"  
                    } else {  
                        $(this).hide(); // Hide card otherwise  
                    }  
                });  
            }  
  
            // Filter cards dynamically while typing in Select2 search  
            $('#brand_name').on('select2:open', function () {  
                const searchBox = document.querySelector('.select2-search__field');  
                searchBox.addEventListener('input', function () {  
                    const searchQuery = this.value.toLowerCase();  
                    filterCardsByName(searchQuery);  
                });  
            });  
  
            // Function to filter cards based on name  
            function filterCardsByName(searchQuery) {  
                $('.brand-card-wrapper').each(function () {  
                    const brandName = $(this).data('name');  
                    if (!searchQuery || brandName.includes(searchQuery)) {  
                        $(this).show(); // Show card if name matches the search query  
                    } else {  
                        $(this).hide(); // Hide card otherwise  
                    }  
                });  
            }  
        });  
    </script>  
  
@stop  
  
  
  
{{--@extends('partner.layouts.app')--}}  
{{--@section('page')--}}  
  
{{--<!-- Header -->--}}  
{{--<div class="row align-items-center brandDown">--}}  
{{--    <div class="d-flex justify-content-between align-items-center header-section">--}}  
{{--        <div class="col-md-8">--}}  
{{--            <h2 class="brandList"><i class="bi bi-star-fill"></i> Brand List <span class="text-muted">({{sizeof($brands)}}/10)</span></h2>--}}  
{{--            <p>See your projects and create new ones under the selected brand.</p>--}}  
{{--        </div>--}}  
{{--        <div class="col-md-2 dropdown" style="text-align: right">--}}  
{{--            <button class="btnColor dropdown-toggle" type="button" id="dropdownMenuButton" data-bs-toggle="dropdown" aria-expanded="false">--}}  
{{--                All Brands List--}}  
{{--            </button>--}}  
{{--            <ul class="dropdown-menu dropdownSize" aria-labelledby="dropdownMenuButton">--}}  
{{--                <li class="dropdown-item d-flex align-items-center brand-item" style="cursor: pointer" data-brand-name="All Brands List">--}}  
{{--                    <span style="margin-left: 10px;">All Brands List</span>--}}  
{{--                </li>--}}  
{{--                @foreach($brands as $brand)--}}  
{{--                    <li class="dropdown-item d-flex align-items-center brand-item" style="cursor: pointer" data-brand-name="{{ $brand->name }}">--}}  
{{--                        <img src="{{ asset($brand->logo ?? 'images/default-logo.png') }}" alt="Brand Logo" class="me-2 brandLogoDetails">--}}  
{{--                        <span style="margin-left: 10px;">{{ $brand->name }}</span>--}}  
{{--                    </li>--}}  
{{--                @endforeach--}}  
{{--            </ul>--}}  
{{--        </div>--}}  
  
{{--        <div class="col-md-2 search-box">--}}  
{{--            <input type="text" id="searchInput" placeholder="Search">--}}  
{{--            <i class="bi bi-search"></i>--}}  
{{--        </div>--}}  
  
{{--    </div>--}}  
  
{{--</div>--}}  
  
{{-- All Brands list start--}}  
{{--<div class="row g-3 align-items-center projectBrand">--}}  
{{--    <div class="col-auto">--}}  
{{--        <label class="control-label brandList">Select Your Brand </label>--}}  
{{--    </div>--}}  
{{--    <div class="col-md-3">--}}  
{{--        <select name="brand_name" onchange="sessionUpdate('brand_id',this.value)" id="brand_name" class="form-select" aria-label="Default select example" style="padding: 8px;margin-bottom: 11px;">--}}  
{{--            <option value="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;All Brands List</option>--}}  
{{--            @foreach($brands as $brand)--}}  
{{--                <option value="{{$brand->id}}" data-image="{{asset( $brand->logo) }}">{{ $brand->name }}</option>--}}  
{{--            @endforeach--}}  
{{--        </select>--}}  
{{--    </div>--}}  
{{--</div>--}}  
{{-- All Brands list end--}}  
  
{{--<script>--}}  
{{--    --}}{{----}}{{--All Brand List with Image Start --}}  
{{--    $(document).ready(function () {--}}  
{{--        $('#brand_name').select2({--}}  
{{--            templateResult: formatOption,--}}  
{{--            templateSelection: formatOption,--}}  
{{--        });--}}  
{{--        function formatOption(option) {--}}  
{{--            if (!option.id) {--}}  
{{--                return option.text;--}}  
{{--            }--}}  
{{--            const imgUrl = $(option.element).data('image');--}}  
{{--            return $(`<span class="custom-option"><img src="${imgUrl}" alt="icon" />${option.text}</span>`);--}}  
{{--        }--}}  
{{--    });--}}  
{{--    --}}{{----}}{{--All Brand List with Image End --}}  
{{--</script>--}}  
  
  
{{--    <!-- Asset Cards Grid -->--}}  
{{--    <div class="row">--}}  
{{--        <!-- Create a Brand Card -->--}}  
  
{{--        <div class="col-md-3 mb-4">--}}  
{{--            <a href="{{ URL::to('/') }}/partner/create-brand" class="card-create" style="text-decoration: none; display: block;">--}}  
{{--                <div class="brand-card brand-card">--}}  
{{--                    <i class="bi bi-plus-circle"></i>--}}  
{{--                    <h5 class="createh5">Create a Brand</h5>--}}  
{{--                    <p>Click here to add an additional brand that you can use to generate assets.</p>--}}  
{{--                </div>--}}  
{{--            </a>--}}  
{{--        </div>--}}  
  
{{--        <!-- Brand Card Example -->--}}  
  
{{--        @foreach($brands as $brand)--}}  
{{--            <div class="col-md-3 mb-4 brand-card-wrapper" data-name="{{ strtolower($brand->name) }}">--}}  
{{--                <a href="{{ URL::to('/') }}/partner/edit-partner-brand/{{$brand->id}}" class="card-create" style="text-decoration: none; display: block;">--}}  
{{--                    <div class="brand-card brands brand-card d-flex flex-column justify-content-between align-items-center text-center">--}}  
{{--                        <img src="{{asset($brand->logo)}}" alt="Brand Logo">--}}  
{{--                        <h5>{{ $brand->name }}</h5>--}}  
{{--                        <p>1 Project Created</p>--}}  
{{--                    </div>--}}  
{{--                </a>--}}  
{{--            </div>--}}  
{{--        @endforeach--}}  
  
{{--        <!-- Additional cards can be added here as needed -->--}}  
{{--    </div>--}}  
  
{{--<script>--}}  
  
{{--    // Search start--}}  
{{--    document.getElementById('searchInput').addEventListener('input', function() {--}}  
{{--        const searchQuery = this.value.toLowerCase(); // Get the search query in lowercase--}}  
{{--        const brandCards = document.querySelectorAll('.brand-card-wrapper'); // Get all brand cards--}}  
  
{{--        brandCards.forEach(function(card) {--}}  
{{--            const brandName = card.getAttribute('data-name'); // Get the brand name from data-name attribute--}}  
  
{{--            // If the brand name matches the search query, show the card, otherwise hide it--}}  
{{--            if (brandName.includes(searchQuery)) {--}}  
{{--                card.style.display = 'block'; // Show card--}}  
{{--            } else {--}}  
{{--                card.style.display = 'none'; // Hide card--}}  
{{--            }--}}  
{{--        });--}}  
{{--    });--}}  
{{--    // Search end--}}  
  
{{--    //--}}  
{{--    document.addEventListener('DOMContentLoaded', function () {--}}  
{{--        // Get all dropdown items--}}  
{{--        const brandItems = document.querySelectorAll('.brand-item');--}}  
{{--        // Get the dropdown button--}}  
{{--        const dropdownButton = document.getElementById('dropdownMenuButton');--}}  
  
{{--        // Add click event listener to each dropdown item--}}  
{{--        brandItems.forEach(item => {--}}  
{{--            item.addEventListener('click', function () {--}}  
{{--                // Get the brand name and ID from the clicked item--}}  
{{--                const brandName = this.getAttribute('data-brand-name');--}}  
{{--                const brandId = this.getAttribute('data-brand-id');--}}  
  
{{--                // Update the dropdown button text--}}  
{{--                dropdownButton.textContent = brandName;--}}  
  
{{--                // Trigger a custom change event--}}  
{{--                const changeEvent = new CustomEvent('brandChange', {--}}  
{{--                    detail: { brandName: brandName, brandId: brandId },--}}  
{{--                });--}}  
{{--                dropdownButton.dispatchEvent(changeEvent);--}}  
{{--            });--}}  
{{--        });--}}  
  
{{--        // Listen for the custom 'brandChange' event--}}  
{{--        dropdownButton.addEventListener('brandChange', function (e) {--}}  
{{--            const searchQuery = e.detail.brandName.toLowerCase() == 'all brands list'?'':e.detail.brandName.toLowerCase();--}}  
{{--            const brandCards = document.querySelectorAll('.brand-card-wrapper'); // Get all brand cards--}}  
  
{{--            brandCards.forEach(function(card) {--}}  
{{--                const brandName = card.getAttribute('data-name'); // Get the brand name from data-name attribute--}}  
{{--                debugger;--}}  
{{--                // If the brand name matches the search query, show the card, otherwise hide it--}}  
{{--                if (brandName.includes(searchQuery)) {--}}  
{{--                    card.style.display = 'block'; // Show card--}}  
{{--                } else {--}}  
{{--                    card.style.display = 'none'; // Hide card--}}  
{{--                }--}}  
{{--            });--}}  
{{--        });--}}  
{{--    });--}}  
  
  
  
{{--</script>--}}  
  
  
{{--@stop--}}
```
## 19. Normal search ( search ar input field a search dibo likhbo then etar sathe card theke miliye jegula mile segula niye asbo )
```html
<div class="col-md-2 search-box">  
    <input type="text" id="searchInput" placeholder="Search">  
        <i class="bi bi-search"></i>  
</div>

@foreach($brands as $brand)  
    <div class="col-md-3 mb-4 brand-card-wrapper" data-name="{{ strtolower($brand->name) }}">  
        <a href="{{ URL::to('/') }}/partner/edit-partner-brand/{{$brand->id}}" class="card-create" style="text-decoration: none; display: block;">  
            <div class="brand-card brands brand-card d-flex flex-column justify-content-between align-items-center text-center">  
                <img src="{{asset($brand->logo)}}" alt="Brand Logo">  
                <h5>{{ $brand->name }}</h5>  
                <p>1 Project Created</p>  
            </div>        </a>    </div>@endforeach
```
## 23. Date Picker
```php
	$startDate = '2025-01-01';
	$endDate = '2025-01-31';
	
	$records = DB::table('your_table_name')
	    ->whereBetween('your_date_column', [$startDate, $endDate])
	    ->get();
	
	return $records;
```
## 24c. foreign Id casecade on update and delete
```js
$table->foreignId('user_id')->constrained()->cascadeOnUpdate()->restrictOnDelete();
$table->foreignId('category_id')->constrained()->cascadeOnUpdate()->restrictOnDelete();
```
## 25. Dropdown hote kono akta Brand select kora then ei brand ar under ar image gulo db hote search kore niye asar code
```php
 public function fetchImageByBrandId(Request $request){  
        try{  
  
//            if($request->brand_id){  
//                $images = PartnerImage::where('ref_id',$request->brand_id)->get()->map(function ($image) {  
//                    $image->image_path = asset('/' . $image->image_path);  
//                    return $image;  
//                });  
//            }else {  
//                $images = PartnerImage::all()->map(function ($image) {  
//                    $image->image_path = asset('/' . $image->image_path);  
//                    return $image;  
//                });  
//            }  // map korle amara normally blade a ${image['image_path']} evabe likhte pari
            if($request->brand_id){  
                $images = PartnerImage::where('ref_id',$request->brand_id)->orderBy('id','desc')->get();  
            }else {  
                $images = PartnerImage::orderBy('id','desc')->get();  
            }  
            return response()->json(['status' => 'success', 'message' => 'Brand Image List','images'=>$images]);  
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
$(document).ready(function () {  
    $('#brand_name_search').on('change',async function () {  
        let brand_id = $(this).val();  
  
        let res = await axios.post('/partner/fetch-brand-image',{brand_id:brand_id});  
        //debugger;  
        const container = document.getElementById('libraryImage');  
  
        document.getElementById('libraryImage').innerHTML = '';  
  
        res.data.images.forEach((image) => {  
            const div = document.createElement('div');  
            div.className = 'col-md-3 col-sm-6';  
            debugger;  
            const image_path = `{{ asset('') }}${image.image_path}`;  
  
            div.innerHTML = `  
            <div class="image-card">                <img                    src="${image_path}"  
                    alt="Image"                    class="img-fluid picker-image"                    onclick="selectLibraryImage(${image_path})"  
                >                <div class="overlay-icon"><i class="bi bi-heart-fill"></i></div>                <a                    href=""                    data-id = "${image['id']}"  
                    class="download-tag"                    style="text-decoration: none !important;">                    Download                </a>                <a                    href=""                    data-id = "${image['id']}"  
                    onclick="return confirm('Are you sure to delete...?');">                    <button class="delete-tag">Delete</button>                </a>            </div>        `;  
            container.appendChild(div);  
        });  
  
    });  
});
```


```js
//        $partner = Auth::user()->partner()->first();  
//        $brands = Brand::where('partner_id',$partner->id)->orderBy('id','desc')->get();
```
## 26c. Button a click kore Modal open hobe then Modal ar Dropdown hote Brand select kora and Image input hisebe newa tapor Save button a click korle DB a save hobe and ei upload kora image tai prothome show korar code
```html
<button onclick="uploadBrandImage()" class="action-btn me-3"><i class="bi bi-upload me-2"></i>Upload</button>  
  
<div class="modal animated zoomIn" id="upload-image-modal" tabindex="-1" aria-labelledby="exampleModalLabel" aria-hidden="true">  
    <div class="modal-dialog modal-dialog-centered modal-md">  
        <div class="modal-content">  
            <div class="modal-header">  
                <h6 class="modal-title" id="exampleModalLabel">Upload Image</h6>  
            </div>            
            <div class="modal-body">  
                <form id="save-form">  
                    <div class="container">  
                        <div class="row">  
                            <div class="col-12 p-1">  
                                <label class="form-label" style="margin-bottom: 1.25rem!important;">Select Brand<span class="text-danger">*</span></label>  
                                <select name="upload_brand_name" id="upload_brand_name" class="form-select brandSelect w-100 brand_name">  
  
                                </select>                            
                            </div>                            
                            <div class="col-12 p-1">  
                                <label class="form-label">Select Image<span class="text-danger">*</span></label>  
                                <input type="file" name="uploadImage" class="form-control uploadImage1" id="uploadLibraryImage">  
                            </div>                        
                        </div>                    
                    </div>                
                </form>            
            </div>            
            <div class="modal-footer">  
                <button id="upload-modal-close" onclick="uploadModalClose()" class="commonCloseButton">Close</button>  
                <button onclick="uploadLibraryImage()" id="save-btn" class="commonSaveButton" >Save</button>  
            </div>        
        </div>    
    </div>
</div>
```

```js
function uploadBrandImage(){  
    document.getElementById('upload-image-modal').style.display = 'block';  
  
    fetchUploadBrand().then(() => {  
        initializeSelect22();  
    });  
}  
  
function uploadModalClose(){  
    document.getElementById('upload-image-modal').style.display = 'none';  
}  

async function fetchUploadBrand() {  
    try {  
        const res = await axios.get('/partner/brand-list');  
        const brandSelect = $('#upload_brand_name');  
  
        // Clear existing options and add a placeholder  
        brandSelect.empty();  
        brandSelect.append('<option value="">&nbsp;&nbsp;&nbsp;&nbsp;All Assets</option>');  
  
        // Populate the dropdown with options  
        res.data.forEach((brand) => {  
            const option = new Option(brand.name, brand.id, false, false);  
            $(option).attr('data-image', '/'+brand.logo); // Add the custom data attribute  
            brandSelect.append(option); // Append the option to the select element  
        });  
  
        // Trigger change event to ensure Select2 updates  
        brandSelect.trigger('change');  
    } catch (error) {  
        console.error('Error fetching brands:', error); // Handle errors gracefully  
    }  
}

function initializeSelect22() {  
	$('#upload_brand_name').select2({  
		templateResult: formatOptions,  
		templateSelection: formatOption,  
		// placeholder: 'All Brands List', // Placeholder for Select2  
		// allowClear: true, // Allow clearing the selection        dropdownParent: $('#upload-image-modal'), // Attach dropdown to the modal to avoid z-index issues  
	});  
	
	const customCSS = `.select2-selection__placeholder { padding-left: 16px; }`;  
	$('<style>').text(customCSS).appendTo('head');  
}

// Custom formatting for Select2 options  
function formatOptions(option) {  
    if (!option.id) {  
        return option.text;  
    }  
    const imgUrl = $(option.element).data('image');  
    return $(  
        `<span class="custom-option">  
        <img src="${imgUrl}" alt="icon" style="width: 20px; height: 20px; margin-right: 8px;" />  
        ${option.text}  
    </span>`  
    );  
}
  
async function uploadLibraryImage(){  
  
    let uploadBrand = document.getElementById('upload_brand_name').value;  
    let uploadLibraryImage = document.getElementById('uploadLibraryImage').files[0];  
  
    if (uploadBrand.trim().length ===0){  
        errorToast('Select Brand Name! ')  
    }  
    else if(!uploadLibraryImage) {  
        errorToast('Image is Required!')  
    }else{  
  
        let formData = new FormData();  
        formData.append('image',uploadLibraryImage);  
        formData.append('brand_id',uploadBrand);  
  
        const config = {  
            headers:{  
                'content-type': 'multipart/form-data'  
            }  
        }  
        // showLoader();  
        let res = await axios.post('/partner/library-image-upload',formData,config );  
        // hideLoader();  
        // debugger;        if(res.data['status'] === 'success'){  
            document.getElementById('upload-modal-close').click();  
  
            $('#libraryImage').prepend(res.data.imagediv);  
            successToast('Image Uploaded successfully')  
            //await getList();  
            document.getElementById('save-form').reset();  
  
        }else {  
            errorToast('Request Fail!')  
        }  
    }  
}
```

```php
    public function libraryImageUpload(Request $request){  
        try {  
            $client = Auth::user();  
            $partner = Partner::where('client_id',$client->id)->first();  
  
            $image = $request->file('image');  
            $imageExtension       = $image->getClientOriginalExtension(); // .png  
            $imageName            = time().'.'.$imageExtension; //394833.png  
            $directory            = 'library-upload-images/';  
            $image->move($directory, $imageName);  
            $imageURL = $directory.$imageName;  
  
//            $newImage = PartnerImage::create([  
//                'partner_id' => $partner->id,  
//                'image_type' =>'food',  
//                'ref_id' =>$request->brand_id,  
//                'is_paid' =>1,  
//                'price' =>100,  
//                'uploaded_by' => $client->id,  
//                'image_path' =>$imageURL,  
//                'image_category_id' => 1,  
//            ]);  
  
            $partnerImage = new PartnerImage();  
            $partnerImage->partner_id = $partner->id;  
            $partnerImage->image_type = 'food';  
            $partnerImage->ref_id = $request->brand_id;  
            $partnerImage->is_paid = 1;  
            $partnerImage->price = 100;  
            $partnerImage->uploaded_by = $client->id;  
            $partnerImage->image_path = $imageURL;  
            $partnerImage->image_category_id = 1;  
            $partnerImage->save();  
  
  
            $downloadRoute = route('download.image', ['filename' => base64_encode($imageURL)]);  
            $deleteRoute = route('partner.uploaded.image.delete', ['id' => $partnerImage->id]);  
            $imageUrl = env('APP_URL').'/'.$imageURL;  
            // Generate the HTML.  
            $html = <<<HTML  
            <div class="col-md-3 col-sm-6">                <div class="image-card">                    <img src="{$imageUrl}" alt="Delicious Food" class="img-fluid picker-image" onclick="selectLibraryImage('{$imageURL}')">  
                    <div class="overlay-icon"><i class="bi bi-heart-fill"></i></div>                    <a href="{$downloadRoute}" class="download-tag" style="text-decoration: none !important;">Download</a>  
                    <a href="{$deleteRoute}" onclick="return confirm('Are you sure to delete...?');">  
                        <button class="delete-tag">Delete</button>                    </a>                </div>            </div>            HTML;  
  
            return response()->json(['status' => 'success', 'message' => 'Partner Image Uploaded Successfully','imagediv'=>$html]);  
        }  
        catch (\Illuminate\Validation\ValidationException $e) {  
            return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
        }  
        catch (\Exception $e) {  
  
            return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500);  
        }  
    }
```
## 28. Moinul vai image file create edit update
```php

// create part
public function createPartnerBrand(Request $request)  
{  
    try {  
        // Validate the input data  
        $validatedData = $request->validate([  
            'name' => 'required|string|max:255',  
            'description' => 'max:1000',  
            'logo' => 'image|mimes:jpeg,png,jpg,gif,webp',  
            'color' => 'max:7',  
            'website_url' => '',  
            'font_name' => '',  
        ]);  
  
        $client = Auth::user();  
        $partner = $client->partner()->first();  
  
        // Add partner_id to the validated data  
        $validatedData['partner_id'] = $partner ? $partner->id : null;  
  
        // Handle logo file upload  
        if ($request->file('logo')) {  
            // If the user uploads a logo, process it  
            $imageUrl = ImageHelper::uploadImage('uploads', $request->file('logo'));  
            $validatedData['logo'] = $imageUrl;  
        } else {  
            // Path to the default logo in the project directory  
            $defaultImagePath = public_path('brand-logo/default_logo.png'); // Adjust path if needed  
  
            // Check if the default image exists            if (!file_exists($defaultImagePath)) {  
                return response()->json(['error' => 'Default logo file not found.'], 404);  
            }  
  
            // Create a File instance for the default logo  
            $file = new File($defaultImagePath);  
  
            // Upload the default logo to the storage  
            $imageUrl = ImageHelper::uploadImage('uploads', $file);  
            $validatedData['logo'] = $imageUrl;  
        }  
  
        // Save the validated data to the Brand model  
        Brand::create($validatedData);  
  
        return redirect()->route('partner.brand')->with('success', 'Brand created successfully!');  
  
    } catch (ValidationException $e) {  
        return response()->json([  
            'message' => 'Validation failed.',  
            'errors' => $e->errors()  
        ], 422);  
    }  
}
```
```php 
// update part
public function updatePartnerBrand(Request $request, $id)  
    {  
  
        // Moinul vaia code  
//        try {  
//  
//            $brand = Brand::findOrFail($id);  
//  
//            $validatedData = $request->validate([  
//                'name' => 'max:255',  
//                'description' => 'max:1000',  
//                'logo' => 'nullable|image|mimes:jpeg,png,jpg,gif,webp', // Logo is nullable for updates  
//                'color' => 'max:7',  
//                'website_url' => '',  
//                'font_name' => '',  
//            ]);  
//  
//            $partner = Auth::user()->partner()->first();  
//            $validatedData['partner_id'] = $partner['id'];  
//  
//            if ($request->hasFile('logo')) {  
//                $image = $request->file('logo');  
//                $imageName = time() . '.' . $image->getClientOriginalExtension();  
//                $directory = 'brand-logo/';  
//                $image->move(public_path($directory), $imageName);  
//                $validatedData['logo'] = $directory . $imageName; // Update with file path  
//            }  
//  
//            $brand->update($validatedData);  
//  
//            return redirect()->route('partner.brand') // Adjust route name as needed  
//            ->with('success', 'Brand updated successfully!');  
//        }  
    // sawon code start        try{  
            // Validate the input data  
            $validatedData = $request->validate([  
                'name' => 'max:255',  
                'description' => 'max:1000',  
                'logo' => 'nullable|image|mimes:jpeg,png,jpg,gif,webp', // Logo is nullable for updates  
                'color' => 'max:7',  
                'website_url' => '',  
                'font_name' => '',  
            ]);  
  
            // Fetch the existing brand record  
            $brand = Brand::findOrFail($id);  
  
            $partner = Auth::user()->partner()->first();  
            $validatedData['partner_id'] = $partner['id'];  
  
            // Handle logo upload if a new logo is provided  
            if ($request->hasFile('logo')) {  
                // Delete the old logo from storage if it exists  
                if ($brand->logo && Storage::disk('s3')->exists($brand->logo)) {  
                    Storage::disk('s3')->delete($brand->logo);  
                }  
  
                // Upload the new image  
                $imageUrl = ImageHelper::uploadImage('uploads', $request->file('logo'));  
                $validatedData['logo'] = $imageUrl;  
            } else {  
                // Check if the logo exists, if not, set the default image  
                if (!$brand->logo) {  
                    // Path to the default logo in the project directory  
                    $defaultImagePath = public_path('brand-logo/default_logo.png'); // Adjust path if needed  
  
                    // Check if the default image exists                    if (!file_exists($defaultImagePath)) {  
                        return response()->json(['error' => 'Default logo file not found.'], 404);  
                    }  
  
                    // Create a File instance for the default logo  
                    $file = new File($defaultImagePath);  
  
                    // Upload the default logo to the storage  
                    $imageUrl = ImageHelper::uploadImage('uploads', $file);  
                    $validatedData['logo'] = $imageUrl;  
                }  
            }  
  
            // Update the brand with the validated data  
            $brand->update($validatedData);  
  
            // Redirect with success message  
            return redirect()->route('partner.brand') // Adjust route name as needed  
            ->with('success', 'Brand updated successfully!');  
        }  
  
        catch (ValidationException $e) {  
  
            return response()->json([  
                'message' => 'Validation failed.',  
                'errors' => $e->errors()  
            ], 422);  
        } catch (\Exception $e) {  
            return redirect()->route('partner.brand')  
                ->with('error', 'An error occurred while updating the brand.');  
        }  
    }
```
```php 
// helper iamge start
 public static function uploadImage($folder, $file)  
    {  
        // sawon code start  
        if ($file instanceof \Illuminate\Http\UploadedFile) {  
            $filePath = 'premiumad/' . $folder . '/' . uniqid() . '.' . $file->getClientOriginalExtension();  
        } else {  
            $extension = pathinfo($file->getRealPath(), PATHINFO_EXTENSION);  
            $filePath = 'premiumad/' . $folder . '/' . uniqid() . '.' . $extension;  
        }  
        $uploaded = Storage::disk('s3')->put($filePath, file_get_contents($file), 'public');  
        return $uploaded ? $filePath : null;  
  
//            Moinul vaia code start  
//        $filePath = 'premiumad/'. $folder . '/' . uniqid() . '.' . $file->getClientOriginalExtension();  
//  
//        // Upload to DigitalOcean Spaces  
//        $uploaded = Storage::disk('s3')->put($filePath, file_get_contents($file), 'public');  
//        return $uploaded ? $filePath : null;  
//        // return $uploaded ? Storage::disk('s3')->url($filePath) : null;  
    }
```
```php 
public function createPartnerBrand(Request $request)  
{  
    try {  
        $validatedData = $request->validate([  
            'name' => 'required|string|max:255',  
            'description' => 'max:1000',  
            'logo' => 'image|mimes:jpeg,png,jpg,gif,webp',  
            'color' => 'max:7',  
            'website_url' => '',  
            'font_name' => '',  
        ]);  
  
        $client = Auth::user();  
        $partner = $client->partner()->first();  
  
        // Add partner_id to the validated data  
        $validatedData['partner_id'] = $partner ? $partner->id : null;  
  
        // Handle logo file upload  
        if ($request->file('logo')) {  
            $imageUrl = ImageHelper::uploadImage('uploads', $request->file('logo'));  
            $validatedData['logo'] = $imageUrl;  
        }  
  
        else {  
            // Path to the default logo in the project directory  
            $defaultImagePath = public_path('brand-logo/default_logo.png'); // Adjust path if needed  
  
            // Check if the default image exists            if (!file_exists($defaultImagePath)) {  
                return response()->json(['error' => 'Default logo file not found.'], 404);  
            }  
  
            // Create a File instance for the default logo  
            $file = new File($defaultImagePath);  
  
            // Upload the default logo to the storage  
            $imageUrl = ImageHelper::uploadImage('uploads', $file);  
            $validatedData['logo'] = $imageUrl;  
        }  
  
        // Save the validated data to the Brand model  
        Brand::create($validatedData);  
  
        return redirect()->route('partner.brand')->with('success', 'Brand created successfully!');  
  
    }  
    catch (ValidationException $e) {  
        return response()->json([  
            'message' => 'Validation failed.',  
            'errors' => $e->errors()  
        ], 422);  
    }  
}
```
## 29c. Date Picker Range dile se onujayi image db hote niye asbe,abar image k delete,download kora jabe ei code diye
![[date_picker_img 1.png]]
```html
<!-- Date Range Picker select button start -->  
	<div class="library-filter action-btn" id="dateRangePickerButton">  
	    <i class="bi bi-calendar-event-fill me-1"></i>  
	    <span id="selectedDateRange">Select Date Range</span>  
	    <i class="bi bi-caret-down-fill ms-1"></i>  
	</div>  
<!-- Date Range Picker select button end -->

// ekhane ese image bosbe start
<div class="image-grid">  
    <div class="row g-4" id="libraryImage">  
  
    </div>
</div>
// ekhane ese image bosbe end

{{--    detele modal start--}}  
    <div class="modal animated zoomIn" id="brand-delete-modal">  
        <div class="modal-dialog modal-dialog-centered">  
            <div class="modal-content">  
                <div class="modal-body text-center">  
                    <h3 class=" mt-3 commonDeleteBtn" >Delete !</h3>  
                    <p class="mb-3">Once delete, you can't get it back.</p>  
                    <input class="d-none" id="brandDeleteID"/>  
                </div>                
                <div class="modal-footer justify-content-end">  
                    <div>                       
	                    <button type="button" id="delete-modal-close" class="commonSaveButton mx-2" data-bs-dismiss="modal">Cancel</button>  
	                    <button onclick="brandItemDelete()" type="button" id="confirmDelete" class="commonCloseButton" >Delete</button>  
                    </div>                
                </div>            
            </div>        
        </div>    
    </div>
{{-- detele modal end--}}
```

```js
// Date Picker Start  
$(document).ready(function () {  
    $('#dateRangePickerButton').daterangepicker({  
        autoUpdateInput: false,  
        locale: {  
            format: 'DD/MM/YYYY',  
            applyLabel: 'Apply',  
            cancelLabel: 'Clear',  
        },  
    });  
    $('#dateRangePickerButton').on('apply.daterangepicker', async function (ev, picker) {  
        const startDate = picker.startDate.format('YYYY-MM-DD');  
        const endDate = picker.endDate.format('YYYY-MM-DD');  
        $('#selectedDateRange').text(picker.startDate.format('DD/MM/YYYY') + ' - ' + picker.endDate.format('DD/MM/YYYY'));  
  
        let res = await axios.post('/partner/fetch-datewise-image', { start_date: startDate, end_date: endDate, });  
          
        const container = document.getElementById('libraryImage');  
        container.innerHTML = '';  
  
        res.data.images.forEach((item) => {  
            const div = document.createElement('div');  
            div.className = 'col-md-3 col-sm-6';  
  
            const image_path = `{{ asset('') }}${item.image_path}`;  
  
            div.innerHTML = `  
                <div class="image-card">                    
	                <img src="${image_path}" alt="Image" class="img-fluid picker-image">  
                    <div class="overlay-icon"><i class="bi bi-heart-fill"></i></div>                    
                    <a href="/partner/download/${btoa(item.image_path)}" data-id="${item['id']}" class="download-tag" style="text-decoration: none !important;">Download</a>  
                    <button data-id="${item['id']}" class="delete-tag brandDeleteBtn">Delete</button>  
                </div>`;  
            container.appendChild(div);  
        });  
        attachDeleteEvent();  
    });  
  
    function attachDeleteEvent() {  
        $('.brandDeleteBtn').on('click', function () {  
            let id = $(this).data('id');  
            $('#brandDeleteID').val(id); 
            $('#brand-delete-modal').modal('show'); 
        });  
    }  
    attachDeleteEvent();  
    
    $('#dateRangePickerButton').on('cancel.daterangepicker', function () {  
        $('#selectedDateRange').text('Select Date Range');  
        $('#imageContainer').empty();  
    });  
});  
// Date Picker End

// image ar moddhe delete button start  
	async function brandItemDelete() {  
	    let id = document.getElementById('brandDeleteID').value;  
	  
	    let res = await axios.post('/partner/partner-upload-image-delete', { id: id });  
	  
	    if (res.data['status'] === 'success') {  
	        document.getElementById('delete-modal-close').click();  
	        successToast('Image Deleted successfully');  
	  
	        const brand_id = $('#brand_name_search').val();  
	        let updatedRes = await axios.post('/partner/fetch-brand-image', { brand_id: brand_id });  
	  
	        const container = document.getElementById('libraryImage');  
	        container.innerHTML = '';  
	  
	        updatedRes.data.images.forEach((item) => {  
	            const div = document.createElement('div');  
	            div.className = 'col-md-3 col-sm-6';  
	  
	            const image_path = `{{ asset('') }}${item.image_path}`;  
	            div.innerHTML = `  
	                <div class="image-card">                    
	                <img src="${image_path}" alt="Image" class="img-fluid picker-image" onclick="selectLibraryImage('${image_path}')">  
	                    <div class="overlay-icon"><i class="bi bi-heart-fill"></i></div>                    
	                    <a href="/partner/download/${btoa(item.image_path)}" class="download-tag" style="text-decoration: none !important;">Download</a>  
	                    <button data-id="${item['id']}" class="delete-tag brandDeleteBtn">Delete</button>  
	                </div>`;  
	            container.appendChild(div);  
	        });  
	  
	        attachDeleteEvent();  
	        function attachDeleteEvent() {  
	            $('.brandDeleteBtn').on('click', function () {  
	                let id = $(this).data('id');  
	                $('#brandDeleteID').val(id);
	                $('#brand-delete-modal').modal('show');
	            });  
	        }  
	    } else {  
	        errorToast('Image Deletion Failed');  
	    }  
	}  
// image ar moddhe delete button end
```

```php 
	// route start
	Route::post('/fetch-datewise-image', [\App\Http\Controllers\Partner\PartnerImageController::class, 'fetchImageByDate'])->name('fetch.datewise.image');
	Route::get('/download/{filename}', [\App\Http\Controllers\Partner\PartnerImageController::class, 'downloadImage'])->name('download.image');
	// route end
	
	// fetch image from database start
	public function fetchImageByDate(Request $request){  
	    $request->validate([  
	        'start_date' => 'required|date',  
	        'end_date' => 'required|date|after_or_equal:start_date',  
	    ]);  
	    $images = PartnerImage::whereBetween('created_at', [$request->start_date, $request->end_date])->get();  
	    return response()->json(['status' => 'success', 'message' => 'Brand Image List','images'=>$images]);  
	}
	// fetch image from database end
	// for download start
	public function downloadImage($filename) {  
	    try{  
	        $decodedParam = base64_decode($filename);  
	        $filePath = public_path($decodedParam);  
	  
	        if (file_exists($filePath)) {  
	            return response()->download($filePath);  
	        }  
	        return back()->withErrors(['error' => 'File not found.']);  
	    }  
	    catch (\Illuminate\Validation\ValidationException $e) {  
	        return response()->json(['error' => 'Validation failed.', 'details' => $e->errors()], 422);  
	    }  
	    catch (\Exception $e) {  
	        return response()->json(['error' => 'An unexpected error occurred.', 'details' => $e->getMessage()], 500);  
	    }  
	} 
// for download end
```
## 30. map function use kora hoyeche ekhane , controller a jodi map function use kora hoy tahole image ar extra address like asset ta ekhan thekei diye deya hoy r jodi controller a map use na kori tahole js a giye assest add kore dite hobe. ekhane comment out korata map ar kaj. ekhane ami map use kori nai js a giye kaj korchi. 
```php
    public function fetchImageByBrandId(Request $request){  
        try{  
  
//            if($request->brand_id){  
//                $images = PartnerImage::where('ref_id',$request->brand_id)->get()->map(function ($image) {  
//                    $image->image_path = asset('/' . $image->image_path);  
//                    return $image;  
//                });  
//            }else {  
//                $images = PartnerImage::all()->map(function ($image) {  
//                    $image->image_path = asset('/' . $image->image_path);  
//                    return $image;  
//                });  
//            }  
            if($request->brand_id){  
                $images = PartnerImage::where('ref_id',$request->brand_id)->orderBy('id','desc')->get();  
            }else {  
                $images = PartnerImage::orderBy('id','desc')->get();  
            }  
            return response()->json(['status' => 'success', 'message' => 'Brand Image List','images'=>$images]);  
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
$('#brand_name_search').on('change', async function () {  

    let brand_id = $(this).val();  
    let res = await axios.post('/partner/fetch-brand-image', { brand_id: brand_id });  
  
    const container = document.getElementById('libraryImage');  
    container.innerHTML = '';  
  
    if (!res.data.images || res.data.images.length === 0) {  
        errorToast('No images available for this Brand.');  
        return;  
    }  
  
    res.data.images.forEach((item) => {  
  
        const div = document.createElement('div');  
        div.className = 'col-md-3 col-sm-6';  
  
        const image_path = `{{ asset('') }}${item.image_path}`;  
  
        div.innerHTML = `  
            <div class="image-card">                
	            <img src="${image_path}" alt="Image" class="img-fluid picker-image" onclick="selectLibraryImage('${image_path}')">  
                <div class="overlay-icon"><i class="bi bi-heart-fill"></i></div>                
                <a href="/partner/download/${btoa(item.image_path)}" data-id="${item['id']}" class="download-tag" style="text-decoration: none !important;">Download</a>  
                <button data-id="${item['id']}" class="delete-tag brandDeleteBtn">Delete</button>  
            </div>        `;  
        container.appendChild(div);  
    });  
});
```
## 31. Laravel single table Migration and Rollback code 
```php

	// specific migration
	php artisan migrate --path=database/migrations/2025_01_11_123456_create_table_name.php
	// specific rollback
	php artisan migrate:rollback --path=/database/migrations/2024_01_01_123456_create_table_name.php
	
```
## 32. Laravel image create code Habib sir
```php
$image = $request->file('image');  
$imageExtension       = $image->getClientOriginalExtension(); // .png  
$imageName            = time().'.'.$imageExtension; //394833.png  
$directory            = 'library-upload-images/';  
$image->move($directory, $imageName);  
$imageURL = $directory.$imageName;
```


