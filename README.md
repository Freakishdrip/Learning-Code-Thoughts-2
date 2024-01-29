# Learning-Code-Thoughts-2
The second phase of HTML,CSS,and JavaScript went surprisingly well. We practiced and learned how to get, post, patch, and delete and I am going to write about all the processes here. The first one GET, is easy said as easy done. You set up a fetch request either inside a useEffect or a useLoader and once the page renders the data from the fetch request is accessed. From there theres only 2 more things you need to do. The first is setting the information that you grabbed into a JSON file format. The next would be to set that to your AllData setter. ex: const [ allData, useAllData ] = useState([]) then, your get is all set up. 
The next one would be a post request. In our post request function we first e.preventDefault(). This is because my post request's data was gathered from a form tag. When a form is submitted the natural behavior of the form is to refresh the page. In this case we dont need that. Next in that same function we declare a const of a newDataObj. We set the id to increment 1 based off of the length of the datalist. Then, set all of the variables inside of the newDataObj to placeholder values. ex
 const newDataObj = {
        id: (allDataObjs.length + 1).toString(),
        parameter: parameter,
        another: another,
        secondParameter: secondParameter,
        name: name,
        image: image,
    }
This is needed for when we do a post request we need to be able to store all of the information that the user submitted in the right place. after our const declaration we have out fetch request for POST. Everything is pretty much the same as get here with a couple exceptions. 
 fetch("http://localhost:4000/data", {
        method: "POST",
        headers: {"Content-type":"application/json"},
        body: JSON.stringify(newDataObj)
    })
    .then((response) => response.json())
    .then((resp) => onAddObj(resp))
    onAddObj(newDataObj)
}
The first difference is that in our body: we need to insert our newDataObj to get stringified so that it is all the same format making it easier to work with. After that we have our standard response into json line. Then we have to pass our response thats now JSON-fied into our onAddData function. This is a callback function that is initally declared in the app.js file. The logic of it is to take in our newDataObj and append it to the end of our dataset by using a spread operator with our inital dataset then add it after. Its good mention with our POST request is that there has to be a way for the form to know which value goes where and I had written that as a const for each variable and a function that would call the setter from the form and insert the new value that the user inputed.

The most challenging of the 3 would be our patch request. It starts by our standard const and function declarations for each variable. 
const [name, setName] = useState(editObj.name) 
function handleName(e){
  setName(e.target.value)
}
Im sure there is a more dynamic way to call this information but this way does work. After we make sure that we are passing the right editObj into our component we can then begin to write our patch request. 
 
 
 function handleEditObj(e){
    e.preventDefault()
    console.log(editObj.id)
    fetch(`http://localhost:4000/cars/${editObj.id}`, {
        method: "PATCH",
        headers: {"Content-Type":"application/json"},
        body: JSON.stringify({ <-- we are still stringifing the response of out body.

          make: make,
          model: model,
          year: year,
          mpg: mpg,
          image: image
        })
    })
    .then((response) => response.json())
    .then((editedOBJ) => { <-- here is where we are turning our edited object 
      console.log(garage)
      console.log(editedCar)
      setGarage(garage.map((car)=>{ <-- here is where we have to map our through each entry of our data set until the id's match
        if(car.id === editedCar.id){
          return editedCar
        }
        else{
          return car
        }
      }))
    )})
}
Don't forget to make sure you are using your setters in the right place! 

Our last operation would be the Delete request. This is by far the easiest to do. first make our function that is called on a button press. Then we call our fetch request. instead of method POST PATCH we insert DELETE here. no need to turn in the response to a JSON type either this time. after the Fetch request all that we need to do is callback to our onDelete function in App.js and give it the id of the object we are deleting. 
heres an example of what our onDelete function looks like using filter to remove the obj. 

function onDelete(id){
  const newObj = allObjs.filter((obj) => obj.id !== id)
  setAllObjs(newObj)
}

the delete functionality is fairly easy to understand. All we are saying with our delete function is that the newObj is set equal to all of the id's except for the one that matches from our delete button pressed earlier. 

This covers all of the GET POST PATCH DELETE functionality that I learned so far. 
