Readme file of Test-Indigital Assignment




************ TASK1 Company User Registration ************

1.Once You Successfully Run Project,You See the Default or Home Page.On Home Page You Will see the Message "Welcome To Test-Indigital" and Also The Counts Of User Which Was Registred in Company.

2.Now If You check Header Of Page, There are Options Or Menu's are Available. 1st.Home,2nd Company Users and 3rd Contact-List.

3.If You Are clicked on Home Menu You Will Remain On Same Page On Which You Come After Successufuly execute project.

4.Then If You Are Clicked On Company Users Menu You will See The List Of Company Users Which was Added and Also there is One Button "Add New User".

5.Now If You Clicked On "Add New User Button" It Will Redirect You On Company User Registration Form.


************ Explaination Of Company User Registration Form Working************

1.You will see http://localhost:8000/create-new-company-user URL 

2.Routing Mechanisam In Web.php.
	i.Location --TEST-Indigital/routes/web.php
	ii.usage --
	Route::get('create-new-company-user', function () 
	{
    return view('company-user-registration');
	});
	iii.return view('company-user-registration') is the view name that we have to show  whenever 
	http://localhost:8000/create-new-company-user will get called.

3.Views 
	i.location--TEST-Indigital/resources/views/company-user-registration.blade.php 
	ii.what is .blade after view name --Blade is the simple, yet powerful templating engine provided with Laravel.
	iii.use of blade template--- for reducing or simplifying the code that you write in your view
	e.g     <?php echo strtoupper('hello') ?>(PHP VIEW .php ) ==   {{strtoupper('hello')}} (BLADE VIEW .blade.php).

	1.create user view
	  i.Simple registration form with fields names
	  ii.I here want to Mention that I create App.blade.php file Where I create Actual Layout Of The Project And That Layout Is Same For All pages, if you check That Layout Of Home Page and Company Users and Create Company User page Is Same Just The Content In Body Of Each Page Will Change and From App.blade.php i call All Js And Css Wants To Used In My Project.
	  iii.CSS Files Used In Project
	  	  - style.css (TEST-Indigital/public/css/style.css) for styling.
          iv.Js Files Used In Project
	  	  - TEST-Indigital/public/js/jquery.validate.min.js & TEST-Indigital/public/js/company-registration-form.js for validation and form submission to server side.
	  v.In app.blade.php view at top include <meta name="csrf-token" content="{{ csrf_token() }}"> this line
	  	  i.Cross-Site-Request-Forgery is used to protect your application from cross-site request forgery (CSRF) attacks. 
	  iv.Re-Captcha Reference-site-https://github.com/anhskohbo/no-captcha
	  	  i. Step 1. Install package
	  	  		composer require anhskohbo/no-captcha
	  	  ii.Step 2. Register the Recaptch service provider
	  	  		TEST-Indigital/config/app.php:
	  	  		Anhskohbo\NoCaptcha\NoCaptchaServiceProvider::class
		  iii.step 3. Get an Secret Key and Site Key 
 				https://www.google.com/recaptcha/admin#list (You Have To Login From You Account Then Only You  Get That Keys)
	  	  iii.Showing a Re-Captcha in a View
			step 1.add this script <script src='https://www.google.com/recaptcha/api.js'></script> in <head> of app.blade.php 
	  	  	step 2.@captcha
	  	  iv.password stregth REF-https://www.formget.com/password-strength-checker-in-jquery/  code added in company-user-registration.js  file.

	 2. Client side Form validation
	 	i.include jquery.validate.min.js for validation.
	 	ii.company-user-registration.js
	 		1.write your form id to validate form -- $("#company-user-registration-form").validate({
	 		2.ignore: 'input[type="hidden"]', to ignore hidden fields
	 		3.rules = write the validation you want inside the rules
	 			 rules: {
    cmp_usr_name: {
      required: true,


    },
    cmp_usr_email: {
      required: true,
      email:true,
      remote: {
    type: 'post',
    url: 'check-user-email-availability',
   
     data: {
        'cmp_usr_email': function () { return $('#cmp_usr_email').val(); }
    },
   dataType: 'json',
    headers: { 'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content') },
      cache: false,
    }
    },  
    cmp_usr_mobile: {
      required: true,
      regex: /^[1-9]{1}[0-9]{9}$/,
      remote: {
    type: 'post',
    url: 'check-user-mobile-availability',
   
     data: {
        'cmp_usr_mobile': function () { return $('#cmp_usr_mobile').val(); }
    },
   dataType: 'json',
    headers: { 'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content') },
      cache: false,
    }

    },  
    cmp_usr_password: {
      required: true,
    },
    retype_password: {
      required: true,
        equalTo: '#cmp_usr_password'
    },
    cmp_name: {
      required: true,
    },
    cmp_usr_designation:{
      required:true,
    },
    cmp_size:{
      required:true,
    },

  },
cmp_usr_email--name of input
required:true--it will make sure that your field should be mandatory before submitting data.you can write require in input too <input type="email" class="form-control" id="cmp_usr_email" placeholder="Enter email" name="cmp_usr_email">

email:true--to get valid email address like atulkadam190@gmail.com

remote: It is used to do server side validation.To check this email id is already exist or not.it returns boolean value from server side.true means user is new and false means existing user.
regex:To validate mobile no.customized validation for that.To use this we have to add this method before validation. 

$.validator.addMethod(
	 "regex",
	 function(value, element, regexp) {
 var check = false;
 return this.optional(element) || regexp.test(value);
 },);

 url:'check-user-mobile-availability',
 In route-Route::post('check-user-email-availability', 'UserController@checkUserEmail');
4.  messages=customized messages 

  cmp_usr_email: {
		required: "Enter User email.",
		email: "Enter valid email.",
		remote: "Email has already been used. Please use another Email address.",
		},
5.errorPlacement=User defined place where error msg will get displayed

  errorPlacement: function(error, element){
 error.appendTo( element.next('.error-message') );
	},

6.submitHandler=Use to submit data after successful validation.

	i.dataString = $('#company-user-registration-form').serialize();
		-The serialize() method creates a URL encoded text string by serializing form values.
		
	ii.$.ajax({ =to submit data.
	iii.type:'POST'
		-For a request-response between a client and server.
		
	iv.data: dataString, it contains all the filds values data like email,mobile.
	
	v.dataType: 'json', serializing and transmitting structured data over a network connection.
	
	vi. headers: { 'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content') },
	
	-to submit form through ajax add the CSRF in the header of the ajax call otherwise system we through 419 status.we added that line in view too.
	
	vii.  cache: false,=By setting the cache property to false jQuery will append a timestamp to the URL, so the browser won't cache it.
	
	viii.beforeSend: function(){  $('#form_submit').html('<i class="fa fa-circle-o-notch fa-spin" style="font-size:18px"></i> Processing');$('#form_submit').attr('disabled','disabled'); },
	     -The action we have to take before submitting data.Here we are changing the button text from submit to processing and  disabling the button.So that user unable to click button more than once.Preventing from multiple entries.
	ix.url: "insert-company-user",

7.Now controller to insert data--- UserController@createUser ---TEST-Indigital/app/Http/Controllers/UserController.php

	i.Server side validations Validation :-Here we are using inbuild validation library of laravel.
	1.$user_fields = Input::only('cmp_usr_name','cmp_usr_email',)=The input fields we have to validate.
	2. $error_message= array('cmp_usr_name.required'=>"Enter User Name.", 'cmp_usr_email.required'=>"Enter User email.")=For customized messages.
	3. $rules = array(
	'cmp_usr_mobile' =>'required','usr_mobile' =>'min:10|max:10|unique:users',
	'cmp_usr_email' =>'required','email' =>'email|unique:users',
	-Specify the validation you want in the rules.
	-Rule::unique=To check data is already exist or not.
	4.$validation_result=Validator:: make($user_fields,$rules,$error_message);=TO validate.
	5.if($validation_result->fails())
	{
	echo json_encode(array('success'=>false,'message'=>$validation_result->errors()->all(),'error_type'=>'user'));  -> if validation fails it will send Error messages throug ajsx response.

	}
	6. else //if validation passes.
	{
	Write insertion code.
	$user =new User;  ->create an object of users model
	For e.g
	   $user->name=trim($req->cmp_usr_name);
	$user->email=trim($req->cmp_usr_email);
	$user->usr_mobile=trim($req->cmp_usr_mobile);

	$user->password=bcrypt(trim($req->cmp_usr_password));
	$user->usr_cmp_name=trim($req->cmp_name);
	$user->usr_designation=trim($req->cmp_usr_designation);
	$user->usr_cmp_size=trim($req->cmp_size);
	$user->usr_status=1;
	//$user->usr_created_at=date('Y-m-d H:i:s');
	$user->save();   //Data stored in Database using this code


	How to store data in session= array('ass_usr_id' => $user->id,'ass_usr_name'  => Input::get('usr_name'),);Session::push('user', $userdata);
	How to get stored values in the session={{ Session::get('user')[0] ['ass_usr_name']}}
	After successful insertion we will send  response to ajax to proceed further in json format.
		-  echo json_encode(array("success"=>true,"message"=>"Added new records.","linkn"=>'users'));
	}
	$req->session()->flash('create_user', 'Company User Is Added Successfully'); //using this command data store in session 

7.Ajax response
	i.response can be either true or false.
	success: function(response)
	{if(response.success==true){ window.location.href=response.linkn;}}=if response is true then we redirect the user to user list view.

************ TASK2 manage Contacts ************	
	
1.http://localhost:8000/ Hit this URL

2.Now click on Contact-List Menu

3.You be redirected to http://localhost:8000/create-new-company-user this URL

	i.This form will to show the Contact List That We are added From Company User Registration Form.
	
	ii.This Will show an Contact List.On this form you will show an Name Of an Contact and Number Of Contact.
	
	iii. On This List View There Is also Two Buttons edit and Update.if you clicked on edit button it will open an Modal.on that modal there are two text boxes one is for Name And Othe Is For Number. You can chane the Name And Number from Here 
	
	iv.there Is also an Save Button Which will update Your Data. 
	
	v.After submitting form
	
        v-a.Route Path:-Controller to Update Data Route::post('update-contact', 'UserController@updateContact');
	
        v.b.Inside Controller
	
	i. try
		{
		    DB::table('users')
		    ->where('id', $request->id)
		    ->update(['name' =>  $request->name,
			    'usr_mobile' => $request->usr_mobile]);
		    $request->session()->flash('update_user', 'user contact Updated successfully');
		    echo json_encode(array("success"=>true,"message"=>"user contact Updated successfully",'linkn'=>"contact-list"));
		}
		catch (Exception $e)
		{
		       echo json_encode(array("success"=>false,"message"=>"Some Error Occured Try Again"));
		}
        ii.The code inside to an try block is used to update to an Data and send success true response message to an user and also an flash message
	iii.The code inside to an catch is give an error message If error or exception Occurs.
      
        vi.if You Clicked on delete button It Will ask you an Confirmation Message If You Confirm,then it will delete you data 
        vi-a. Route Path:-Controller to delete Data Route::post('delete-contact', 'UserController@deleteContact');
         vi-b.Inside Controller
		  i.try
		{
		    $user = User::find($request->id);    
		    $user->delete();
		    $request->session()->flash('delete_user', 'user contact Deleted successfully');
		    echo json_encode(array("success"=>true,"message"=>"user contact deleted successfully",'linkn'=>"contact-list"));
		}
		catch(Exception $e)
		{
		    echo json_encode(array("success"=>false,"message"=>"Some Error Occured Try Again"));
		}
		ii.The code inside to an try block is used to delete to an Data and send success true response message to an user and also an flash message.
		iii.The code inside to an catch is give message If error or exception Occurs.
		iv code in javascript
		   onclick of delete button Call An function

		    function delete_contact(id)
	{
	   var x = confirm("Are you sure you want to delete?");
	    if (x)
	    {
	      data={
		id:id
	      },
	      $.ajax({
	      type: "POST",
	      url: "delete-contact",
	      data: data,
	      dataType: 'json',
	      headers: { 'X-CSRF-TOKEN': $('meta[name="csrf-token"]').attr('content') },
	      cache: false,

	   success: function(response)
	   {

	    if(response.success==true)
	    {
	      window.location.href=response.linkn;

	    }
	    else{
	      alert(response.message);
		location.reload();
	    }
	   }

	});


	    } 
	    else
	    {
	       return false;
	    }

	}

















