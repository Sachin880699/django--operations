# _____________________ Views.py ________________________

from django.shortcuts import render , HttpResponse
from.models import *
from django.http import JsonResponse

def home(request):
    animal_obj = Animal.objects.all()
    animal_list_obj = AnimalList.objects.all()
    context  = {
        "animal_obj":animal_obj,
        "animal_list_obj":animal_list_obj
    }
    return render(request,'home.html',context)

import json
def animal_list_filter(request):
    if request.method == 'POST':
        try:
            animal_id   = request.POST.get("select_animal_obj")
            raw_list = []
            animal_obj = Animal.objects.get(id = animal_id)
            for rec in animal_obj.breed.all():
                json_dict = {}
                json_dict['id'] = rec.id
                json_dict['breed_name'] = rec.breed_name
                raw_list.append(json_dict)
            return HttpResponse(json.dumps(raw_list))
        except:
            context = {"message": False}
            return HttpResponse(json.dumps(context))

def add_new_animal(request):
    if request.method == "POST":
        animal_id       = request.POST.get("animal_id")
        breed_id        = request.POST.get("breed_id")
        date            = request.POST.get("date")
        animal_obj      = Animal.objects.get(id = animal_id)
        breed_obj       = Breed.objects.get(id = breed_id)
        AnimalList.objects.create(
            name        = animal_obj,
            breed       = breed_obj,
            date        = date
        )
        try:
            raw_list = []
            for rec in AnimalList.objects.all():
                json_dict = {}
                json_dict['id'] = rec.id
                json_dict['name'] = rec.name.name
                json_dict['breed'] = rec.breed.breed_name
                json_dict['created_date'] = str(rec.date)
                raw_list.append(json_dict)
            return HttpResponse(json.dumps(raw_list))
        except Exception as e:
            context = {"message": False}
            return HttpResponse(json.dumps(context))


def delete_animal(request):
    if request.method == "POST":
        animal_id       = request.POST.get("animal_id")
        AnimalList.objects.get(id = animal_id).delete()
    try:
        raw_list = []
        for rec in AnimalList.objects.all():
            json_dict = {}
            json_dict['id'] = rec.id
            json_dict['name'] = rec.name.name
            json_dict['breed'] = rec.breed.breed_name
            json_dict['created_date'] = str(rec.date)
            raw_list.append(json_dict)
        return HttpResponse(json.dumps(raw_list))
    except:
        context = {"message": False}
        return HttpResponse(json.dumps(context))




# ____________________ home.html _________________________


<!doctype html>
<html lang="en">
   <head>
      <meta charset="utf-8">
      <meta name="viewport" content="width=device-width, initial-scale=1">
      <title>Bootstrap demo</title>
      <link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-Zenh87qX5JnK2Jl0vWa8Ck2rdkQ2Bzep5IDxbcnCeuOxjzrPF/et3URy9Bv1WTRi" crossorigin="anonymous">
   </head>
   <body>
      <h1>Animals</h1>
      <div class="container">
         <table class="table">
            <thead>
               <tr>
                  <th scope="col">ID</th>
                  <th scope="col">Animal</th>
                  <th scope="col">Breed</th>
                  <th scope="col">Date</th>
                  <th scope="col">Action</th>
               </tr>
            </thead>
            <tbody id = "table_body_id">
               {% for res in animal_list_obj %}
               <tr>
                  <th scope="row">{{res.id}}</th>
                  <td>{{res.name.name}}</td>
                  <td>
                     {{res.breed.breed_name}}
                  </td>
                  <td>{{res.date}} </td>
                  <td><button class="btn btn-danger" id="delete_button" onclick="delete_animal(this)" value="{{res.id}}">Delete</button> </td>
               </tr>
               {% endfor %}
            </tbody>
         </table>
         <button class="btn btn-primary" onclick="display_add_form()">New</button>
         <br>
         <br>
         <div id="add_form" style="display:none;">
            <form id="animal_form">
               <label >
                  <select class="form-control" onchange="display_breed()" id="select_animal_id" >
                     <option>Select Animal</option>
                     {% for res in animal_obj %}
                     <option value="{{res.id}}">{{res.name}}</option>
                     {% endfor %}
                  </select>
                  Animal
               </label>
               <label id="breed_dropdown" style="display:none;width:124px;">
                  <select class="form-control" onchange="display_date()" id="breed_id">
                     <option>Select Breed</option>
                     {% for res in animal_obj %}
                     <option>{{res.name}}</option>
                     {% endfor %}
                  </select>
                  Breed
               </label>
               <label style="display:none;width:124px;" id="date_id">
               <input type="date" id="animal_date_id" name="breed_name" class="form-control">
               Breed
               </label>
            </form>
            <button class="btn btn-primary" id="submit_form_button_id">Submit</button>
            <button class="btn btn-warning" onclick="cancel_form()">Cancel</button>
         </div>
      </div>
      <script src="https://cdn.jsdelivr.net/npm/bootstrap@5.2.2/dist/js/bootstrap.bundle.min.js" integrity="sha384-OERcA2EqjJCMA+/3y+gxIOqMEjwtxJY7qPCqsdltbNJuaOe923+mo//f6V8Qbsw3" crossorigin="anonymous"></script>
      <script src="https://ajax.googleapis.com/ajax/libs/jquery/3.6.0/jquery.min.js"></script>
      <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
      <script>
         function display_add_form(){
         document.getElementById("add_form").style.display = 'block';
         }
         function cancel_form(){
         document.getElementById("add_form").style.display = 'none';
         document.getElementById("breed_dropdown").style.display = 'none';
         document.getElementById("date_id").style.display = 'none';
         }
         function display_breed(){
         document.getElementById("breed_dropdown").style.display = 'block';
         }
         function display_date(){
         document.getElementById("date_id").style.display = 'block';
         }

         $("#select_animal_id").on('change', function () {
                var select_animal_obj = $('#select_animal_id').val();
                $.ajax({
                    type: 'POST',
                    dataType: 'JSON',
                    url: "{% url 'animal_list_filter' %}" ,
                    data: {select_animal_obj: select_animal_obj, csrfmiddlewaretoken: '{{ csrf_token }}'},
                    success: function (data)
                    {
                        if (data['message'] === false) {
                            $('#breed_id').empty();
                            $('#breed_id').append('<option value="">Please Select</option>');
                        } else {
                            $('#breed_id').empty();
                            $('#breed_id').append('<option value="">Please Select</option>');
                            for (var i = 0; i < data.length; i++) {
                                $('#breed_id').append('<option value="' + data[i].id + '">' + data[i].breed_name + '</option>');
                            }
                        }
                    }
                });
            });



         $("#submit_form_button_id").on('click', function () {
                var animal_id = $('#select_animal_id').val();
                var breed_id = $('#breed_id').val();
                var date = $('#animal_date_id').val();

                $.ajax({
                    type: 'POST',
                    dataType: 'JSON',
                    url: "{% url 'add_new_animal' %}" ,
                    data: {animal_id: animal_id ,breed_id:breed_id ,date:date, csrfmiddlewaretoken: '{{ csrf_token }}'},
                    success: function (data)
                    {
                        if (data['message'] === false) {
                            $('#table_body_id').empty();
                        } else {
                            $('#table_body_id').empty();
                            for (var i = 0; i < data.length; i++) {

                                $('#table_body_id').append('<tr><th scope="row">' + data[i].id + '</th><td> ' + data[i].name + '</td><td> ' + data[i].breed + ' </td><td> ' + data[i].created_date + ' </td><td><button class="btn btn-danger" id="delete_button" onclick="delete_animal(this)" value= ' + data[i].id + '> Delete </button> </td></tr>');
                            }
                        }
                    }
                });
            document.getElementById("animal_form").reset();
            });

         function delete_animal(animal_id){
         $.ajax({
                    type: 'POST',
                    dataType: 'JSON',
                    url: "{% url 'delete_animal' %}" ,
                    data: {animal_id: animal_id.value , csrfmiddlewaretoken: '{{ csrf_token }}'},
                    success: function (data)
                    {
                        if (data['message'] === false) {
                            $('#table_body_id').empty();
                        } else {
                            $('#table_body_id').empty();
                            for (var i = 0; i < data.length; i++) {
                                $('#table_body_id').append('<tr><th scope="row">' + data[i].id + '</th><td> ' + data[i].name + '</td><td> ' + data[i].breed + ' </td><td> ' + data[i].created_date + ' </td><td><button class="btn btn-danger" id="delete_button" onclick="delete_animal(this)" value= ' + data[i].id + '> Delete </button> </td></tr>');
                            }
                        }
                    }
                });
         }

      </script>
   </body>
</html>
