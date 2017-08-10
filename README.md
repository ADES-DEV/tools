Стилизация checkbox на чистом CSS

Заключаем чекбокс и лейбл в контейнер

<div class="checkbox">
  <input type="checkbox" name="checkbox" value="Y">
	<label for="checkbox">Запомнить меня на этом компьютере</label>
</div>


<style>
.checkbox input[type="checkbox"] {
    opacity: 0;
}
.checkbox label::before{
    content: "";
    display: inline-block;
    
    height: 16px;
    width: 16px;
    
    border: 1px solid;   
}
.checkbox label::after {
    content: "";
    display: inline-block;
    height: 6px;
    width: 9px;
    border-left: 2px solid;
    border-bottom: 2px solid;
    
    transform: rotate(-45deg);
}
.checkbox label {
    position: relative;
    font-weight: normal;
    margin: 0 0 .6em 5px;
    font-weight: normal;
    font-size: .9em;
    text-transform: lowercase

}
.checkbox label::before,
.checkbox label::after {
    position: absolute;
}
.checkbox label::before {
    top: 1px;
    left: -20px; 
}
.checkbox label::after {
    top: 5px;
    left: -16px;
}
.checkbox input[type="checkbox"] + label::after {
    content: none;
}
.checkbox input[type="checkbox"]:checked + label::after {
    content: "";
}
.checkbox input[type="checkbox"]:focus + label::before {
    outline: rgb(59, 153, 252) auto 5px;
}
</style>
