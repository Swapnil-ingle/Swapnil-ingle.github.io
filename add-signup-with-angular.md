### Angular project: Adding sign-up functionality

#### Initialize a new angular project

1. `ng new <app_name>`
2. `cd <app_name>`
3. Open in 'Visual Studios'
4. run '`ng serve`' The app will start at localhost:4200

#### Create a header component

0. Run: `ng g c header`
1. Fill the header.component.html and header.component.css file
2. This will have a `'"routerLink = '/sign-up'"` (Add app.router.ts in `routes = [path: '', component = signupComponent]`)
3. Add `<router-outlet></router-outlet>` in the app.component.html file.
4. Add RoutingModule (./app-routing.module) to app.module.ts

#### Create a sign-up component: ng g c auth/signup

1. Fill the signup.component.html and signup.component.css file
2. The HTML file will have a `<form></form>`
3. Add the `[formGroup]='<formGroupName>'` to the `<form [formGroup]='<formGroupName>'></form>` tag
4. Add the `[formControlName]="'email'"` or `[formControlName]="'username'"` or `[formControlName]="'password'` to the form fields
5. Add 'ReactiveFormsModule' to app.module.ts

#### Add the formControl to the signup.compotent.ts file (Making the form smarter)

1. Add 'signUpForm: FormGroup;' as global parameter in the SignUpComponent;
2. Under ngOnInit() { ... } method (This method is invoked as soon as some value is entered in the input type fields) add 
> `this.signUpForm = new FormGroup ({
> username: new FormControl('', Validators.required),
> email: new FormControl('', [Validators.required, Validators.email]),
> password: new FormControl('', Validators.required),
> })`
3. Now, that the validators are setup, add custom msg and checking the input form-fields before the user clicks submit. Add new 
`"<span *ngIf="!signUpForm.get('email').valid && signUpForm.get('email').touched">Please enter valid <username/password/email> field</span>"`
4. Now add the submit button funtionality in `<form [formGroup]="<formGroupName>" (ngSubmit)="signup()"></form>`

#### Add the authService that we'll inject in the signUp.component.ts file.

1. Create a new service `ng g s auth/shared/auth` *(We're creating inside shared as all the auth components (e.g; signUp, logout, etc) would use the same authService)*
2. Create a new DTO for signUpRequest: Inside signUpComponent add a new file signup-request.payload.ts
> `export interface SignupRequestPayload {
> username: string;
> password: string;
> email: string;
> }`
3. In app.module.ts add 'httpClientModule' (from '@angular/common/http') - We need to use this in the auth.service 
4. In the constructor add the (private httpClient: HttpClient)
5. Add the below method in this service
`signUp(signUpRequestPayload: SignUpRequestPayload) : Observable<any> {
  return this.httpClient.post("http://localhost:8080/reddit/api/auth/signup", signUpRequestPayload, {responseType: 'text'})
}`.
6. Now, we can use this signUp method from our signupComponent.ts file.

#### Inject the authService that we just created in out signUpComponent.ts constructor...

1. ...and declare the SignupRequestPayload as a global variable;
2. Inside the constructor init the signupRequestPayload with default values.
3. e.g; 
> `constructor(private authService: AuthService) {
> this.signupRequestPayload = {
> username: '',
> password: '',
> email: ''
> }
> }`

#### Add the signUp() method in the signUpComponent.ts file 
> (Which is invoked from the `<form (ngSubmit)="signup()"></form>` place)

0. As we already have signupRequestPayload and singupForm initialized, we can go ahead and add the below method.
1. 
> `signup() {
> this.signupRequestPayload.username = this.signupForm.get('username').value;
> this.signupRequestPayload.email = this.signupForm.get('email').value;
> this.signupRequestPayload.password = this.signupForm.get('password').value;
> this.authService.signUp(this.signupRequestPayload)
> .subscribe(data => {
> console.log(data)
> });
> }`

#### Done!

This will now call the backend signup API and add a user.
