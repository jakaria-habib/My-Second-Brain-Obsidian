### 1. Form ar vetor jekono button thakle click korle submit hoye jabe kintu close button submit na howar jonno button type= button dite hoy.
```html
<form>
	<button type="submit" class="commonSaveButton">Update</button>
	<button type="button" class="commonCloseButton" id="modal-close" data-bs-dismiss="modal" aria-label="Close">Close</button>  
</form>
``` 
### 2.  Button ar vetor jekono icon like plus icon center a bosanor code 
```html
<button class="createBrandButton"><i class="bi bi-plus-circle"></i></button>
.createBrandButton {  
    background-color: #2f322c;  
    color: #fff;  
    padding: 8px 16px;  
    border-radius: 5px;  
    width: 40px;  
    height: 38px;  
    margin-top: 3px;  
    text-align: center;  
    display: flex;  
    justify-content: center;  
    align-items: center;  
}
```
### 3. Modal ar moddhe close button or cross sign use ar code
```html 
<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
```
### 4.   Modal ar jonno CSS code
<style>  
    .modal-content {  
        transition: transform 0.3s ease, opacity 0.3s ease;  
    }  
    .modal.fade .modal-dialog {  
        transform: translateY(-10%);  
        opacity: 1;  
    }  
    .modal.show .modal-dialog {  
        transform: translateY(0);  
        opacity: 1;  
    }  
</style>
## 5. class ar vetore float-end likhle last a chole jabe.
### 6. Ternary and Null coalescing operator
```php
// null coalescing operator
// Null Coalescing Operator বা নাল কোঅ্যালেসিং অপারেটর এর বাংলা অর্থ হলো একটি অপারেটর যা কোনো ভেরিয়েবলের মান যদি null (অনির্ধারিত) হয়, তবে সেটির জন্য একটি ডিফল্ট মান প্রদান করে।
$request->image ?? null; er mane holo input field a ki image deya hoyeche kina jodi image deya hoye thake tahole image ta nibe ar jodi na diye thake tahole null hisebe nibe.
$val = $request->image ?? 'default_image.jpg';

// ternary operator/ condition 
$user->image ? 'nullable' : 'required'; er mane holo user ar moddhe image ki ache, thakle nullable r na thakle required. eta akta condition syntax
```
### 7. Select Option dropdown a Down Arrow sign paoar jonno select ar class a "form-select" use korte hobe r select ar style a "appearance:auto" kore dileo arrow sign paoa jay. select ar class a jodi form-control nei tokhn manually "appearance:auto" kore dileo arrow sing paoa jabe.
```html
<div class="mb-3">  
    <label for="is_paid" class="form-label">Paid Status<span class="text-danger">*</span></label>  
    <select class="form-select" name="is_paid" style="cursor: pointer;appearance: auto;">  
        <option value="" class="text-muted" selected disabled>-- Paid Status --</option>  
        <option value="1">Premium</option>  
        <option value="0">Free</option>  
    </select>    
</div>

// example
<select class="form-select" name="is_paid">  
<select class="form-control" name="is_paid" style="appearance: auto;">  
```
## 8. Computer Shortcut
```html
1. ctrl W -> dile current tab off hoye jabe
2. alt Tab -> dile kon kon tab on ache dekha jabe and move kora jabe
3. ctrl WW -> dile laravel ar puro function select kora jabe
```
## 9. Error Analysis for Laravel
```php
// MassAssignmentException: Model a fillable property add na korle ei error dey
Illuminate\Database\Eloquent\MassAssignmentException: Add [name] to fillable property to allow mass assignment on [App\Models\University]. in file F:\projects\bizzyv2\vendor\laravel\framework\src\Illuminate\Database\Eloquent\Model.php on line 525
```