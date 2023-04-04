<script lang="ts">
  import moment from 'moment';
  import arrow from './assets/images/icon-arrow.svg'
  import CustomInput from "./components/CustomInput.svelte";

  const inputInfo = [{
    label: 'DAY',
    placeholder: 'DD',
    max: 31,
    error: 'This field is required'
  }, {
    label: 'MONTH',
    placeholder: 'MM',
    max: 12,
    error: 'This field is required'
  }, {
    label: 'YEAR',
    placeholder: 'YYYY',
    max: new Date().getFullYear(),
    error: 'This field is required'
  }]

  const age = {
    day: 0,
    month: 0,
    year: 0
  }

  let years, months, days = 0;

  function handleDate(e: CustomEvent) {
    age[e.detail.field] = e.detail.number;
    const date = new Date(`${age.month}/${age.day}/${age.year}`);
    date.setFullYear(age.year);
    if (age.day && age.month && age.year) {
      const inputs = document.getElementsByTagName('form')[0].getElementsByClassName('custom-input');
      if (age.day !== date.getDate() || age.month !== date.getMonth() + 1 || age.year !== date.getFullYear()) {
        document.getElementsByTagName('form')[0].getElementsByTagName('p')[3].textContent = 'Must be a valid date';
        document.getElementsByTagName('form')[0].getElementsByTagName('p')[3].style.display = 'block';
        for (let i = 0; i < inputs.length; i++) {
          inputs[i].getElementsByTagName('label')[0].style.color = 'var(--light-red)';
          inputs[i].getElementsByTagName('input')[0].style.outlineColor = 'var(--light-red)';
          inputs[i].getElementsByTagName('input')[0].style.borderColor = 'var(--light-red)';
          inputs[i].getElementsByTagName('input')[0].style.caretColor = 'var(--light-red)';
        }
      } else {
        inputInfo[0].error = 'This field is required';
        document.getElementsByTagName('form')[0].getElementsByTagName('p')[3].textContent = 'This field is required';
        document.getElementsByTagName('form')[0].getElementsByTagName('p')[3].style.display = '';
        for (let i = 0; i < inputs.length; i++) {
          inputs[i].getElementsByTagName('label')[0].style.color = '';
          inputs[i].getElementsByTagName('input')[0].style.outlineColor = '';
          inputs[i].getElementsByTagName('input')[0].style.borderColor = '';
          inputs[i].getElementsByTagName('input')[0].style.caretColor = '';
        }
        const start = moment(date);
        const end = moment();
        const duration = moment.duration(end.diff(start));
        years = duration.years();
        months = duration.months();
        days = duration.days();
      }
    }
  }
</script>

<main>
  <form>
    {#each inputInfo as info}
      <CustomInput bind:error={info.error} label={info.label} placeholder={info.placeholder} max={info.max} on:message={(e) => handleDate(e)}></CustomInput>
    {/each}
    <img src={arrow} alt="Arrow">
    <p>Must be a valid date</p>
  </form>
  <div id='result'>
    <h1><span>{years ? years : '--'}</span> years</h1>
    <h1><span>{months ? months : '--'}</span> months</h1>
    <h1><span>{days ? days : '--'}</span> days</h1>
  </div>
</main>

<style>
main {
  padding: 4rem;
}
  form {
    position: relative;
    display: flex;
    justify-content: flex-start;
    width: calc(100% - 3rem);
    padding-bottom: 4rem;
    border-width: 0;
    border-style: solid;
    border-block-end-width: 0.1rem;
    border-block-end-color: var(--light-grey);
    margin-block-end: 3rem;
  }
    form p {
      width: 30%;
      display: none;
      font-family: Poppins-Italic, sans-serif;
      color: var(--light-red);
      position: absolute;
      top: 6rem;
      left: 0;
    }

    form img {
      width: 3rem;
      position: absolute;
      background-color: var(--purple);
      border-radius: 50%;
      padding: 1rem;
      bottom: 0;
      right: 0;
      transform: translate(3rem, 50%);
    }
      form img:hover {
        cursor: pointer;
        background-color: var(--off-black);
      }

  #result {
    font-size: 3rem;
    font-family: Poppins-ExtraBoldItalic, sans-serif;
  }
    #result h1 {
      margin-block: 0;
    }
    #result h1 span {
      overflow-wrap: break-word;
      color: var(--purple)
    }
@media screen and (max-aspect-ratio: 1) and (max-width: 767px) {
  main {
    padding: 0;
  }

    form {
      justify-content: space-between;
      width: 100%;
      padding-bottom: 5rem;
    }
      form p {
        font-size: 0.7rem;
        top: 5rem;
      }

      form img {
        width: 2rem;
        right: 50%;
        transform: translate(50%, 50%);
      }

    #result {
      font-size: 1.5rem;
    }
}
</style>
