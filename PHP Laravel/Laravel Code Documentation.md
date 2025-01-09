
#### 1. Form ar vetor jekono button thakle click korle submit hoye jabe kintu close button submit na howar jonno button type= button dite hoy.
```html
<form>
	<button type="submit" class="commonSaveButton">Update</button>
	<button type="button" class="commonCloseButton" id="modal-close" data-bs-dismiss="modal" aria-label="Close">Close</button>  
</form>
``` 
### 2.  plus icon center a bosanor jonno 

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
### 3. modal ar moddhe close ba cross sign use ar code
```html 
<button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
```

4.   
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
