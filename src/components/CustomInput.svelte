<script lang="ts">
    import {createEventDispatcher} from 'svelte';

    export let label: string;
    export let placeholder: string;
    export let error: string;
    export let max: number;

    let errorStatement = error;

    const dispatch = createEventDispatcher();

    function preventInvalidLetters(e: KeyboardEvent, regexr: RegExp) {
        if (!regexr.test(e.key) && e.key !== 'Tab' && e.key !== 'Backspace' && e.key !== 'Delete' && e.key !== 'F5') {
            e.preventDefault();
        }    
    }

    function handleFocusOut(e: FocusEvent & { currentTarget: EventTarget & HTMLInputElement}) {
        const value = parseInt(e.currentTarget.value);
        if (isNaN(value)) {
            errorStatement = 'This field is required';
            e.currentTarget.parentElement.getElementsByTagName('p')[0].style.display = 'block';
            e.currentTarget.parentElement.getElementsByTagName('label')[0].style.color = 'var(--light-red)';
            e.currentTarget.style.caretColor = 'var(--light-red)';
            e.currentTarget.style.borderColor = 'var(--light-red)';
            e.currentTarget.style.outlineColor = 'var(--light-red)';
        } else if (value > max) {
            errorStatement = 'Must be a valid ' + label.toLowerCase();
            e.currentTarget.parentElement.getElementsByTagName('p')[0].style.display = 'block';
            e.currentTarget.parentElement.getElementsByTagName('label')[0].style.color = 'var(--light-red)';
            e.currentTarget.style.caretColor = 'var(--light-red)';
            e.currentTarget.style.borderColor = 'var(--light-red)';
            e.currentTarget.style.outlineColor = 'var(--light-red)';
        } else {
            errorStatement = '';
            e.currentTarget.parentElement.getElementsByTagName('label')[0].style.color = '';
            e.currentTarget.style.caretColor = '';
            e.currentTarget.style.borderColor = '';
            e.currentTarget.style.outlineColor = '';
            dispatch('message', {
                number: value,
                field: label.toLocaleLowerCase()
            })
        }
    }
</script>

<div class="custom-input">
    <label for={label}><small>{label}</small></label>
    <input 
        id={label}
        placeholder={placeholder}
        on:keydown={(e) => preventInvalidLetters(e, /^[0-9]+$/)} maxlength={label === 'YEAR' ? 4 : 2}
        on:focusout={handleFocusOut}
    >
    <p>{errorStatement}</p>
</div>

<style scoped>
    .custom-input {
        position: relative;
        max-width: 25%;
        margin-right: 2rem;
    }
        .custom-input label {
            display: block;
            letter-spacing: 0.2rem;
            font-family: Poppins-Bold;
            color: var(--smokey-grey);
        }

        .custom-input input {
            padding: 1rem;
            border-radius: 0.5rem;
            border-style: solid;
            border-width: 0.1rem;
            font-family: Poppins-Bold;
            color: var(--off-black);
            font-size: 2rem;
            width: calc(100% - 2rem);
        }
            .custom-input input:hover{
                cursor: pointer;
            }

            .custom-input input::placeholder {
                color: var(--smokey-grey);
            }

            .custom-input input:focus{
                caret-color: var(--purple);
                outline-style: solid;
                outline-width: 0.1rem;
                outline-color: var(--purple);
            }

        .custom-input p {
            position: absolute;
            top: 6rem;
            display: none;
            font-family: Poppins-Italic, sans-serif;
            color: var(--light-red);
        }
@media screen and (max-aspect-ratio: 1) and (max-width: 767px) {
    .custom-input {
        max-width: 30%;
        margin-right: 0;
    }
        .custom-input label {
            font-size: 0.8rem;
        }
        .custom-input input {
            font-size: 1rem;
        }
        .custom-input p {
            top: 5rem;
            font-size: 0.7rem;
        }
}
</style>