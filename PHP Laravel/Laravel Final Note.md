## 1. Java Script Notification Code:

1  Add link in the head of html template and Download this link and script from website
```Link
<link href="{{asset('css/toastify.min.css')}}" rel="stylesheet" />
<script src="{{asset('js/jquery-3.7.0.min.js')}}"></script>
<script src="{{asset('js/toastify-js.js')}}"></script>
```
2  Add this code in config.js ( Success and Error Toast  JavaScript code )
```js code 
This is Rabbil vai code:
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

## 2. Laravel Notification Code do it not yet
## 3. JavaScript Show Loader and Hide Loader

1  Add this in Laravel blade file  
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

## 5. Dropdown List with Image code
```Html
{{-- Html code Start--}}  
<div class="col-md-4 align-items-center">  
    <label class="control-label dashboardBrandList">Select Your Brand </label>  
    <select name="brand_name" onchange="sessionUpdate('brand_id',this.value)" id="brand_name" class="form-select" aria-label="Default select example" style="padding: 8px;margin-bottom: 11px;">  
        <option value="">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;All Brands List</option>  
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
