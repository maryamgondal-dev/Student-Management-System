# Student-Management-System
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */
package newcode;
/*
 * To change this license header, choose License Headers in Project Properties.
 * To change this template file, choose Tools | Templates
 * and open the template in the editor.
 */




import java.util.ArrayList;
import java.util.Scanner;


class Course{
    private  String courseName;
    private String courseId;
    private  ArrayList<Student> students;
    private ArrayList<Grade> grades;
    private  Department department;
    private  String studentType;

    public Course() {
    }
    

    public Course(String courseName, String courseId, Department department, String studentType){
        this.courseName=courseName;
        this.courseId=courseId;
        this.students=new ArrayList<>();
        this.grades=new ArrayList<>();
        this.department=department;
         this.studentType=studentType;
    }

    public String getCourseName(){
        return courseName;
    }

    public String getCourseId(){
        return courseId;
    }

    public Department getDepartment(){
        return department;
    }

    public String getStudentType(){
        return studentType;
    }

    public void enrollStudent(Student student){
        if(!student.getStudentType().equals(studentType)){
            System.out.println("This course is only for "+studentType+" students.");
        }else if(!student.getDepartment().getName().equals(department.getName())){
            System.out.println("This student is not from the "+department.getName()+" department.");
        }else{
            students.add(student);
            System.out.println("Student enrolled in course successfully.");
        }
    }

    public void assignGrade(Student student, int grade){
        for(Student s:students){
           if(s.getId().equals(student.getId())){
                grades.add(new Grade(student, this, grade));
                System.out.println("Grade assigned successfully.");
                return;
            }
        }
        System.out.println("Student not found in this course.");
    }

    public void listStudents(){
        if(students.isEmpty()){
            System.out.println("No students enrolled in this course.");
        }else{
            students.forEach((student) -> {
                System.out.println(student.details());
            });
        }
    }

    public void listGrades(){
        if(grades.isEmpty()){
            System.out.println("No grades assigned in this course.");
        }else{
            grades.forEach((grade) -> {
                System.out.println(grade.details());
            });
        }
    }

  public void listPassedStudents() {
    boolean hasPassedStudents = false;
    System.out.println("Passed Students:");
    
    for (int i = 0; i < grades.size(); i++) {
        Grade grade = grades.get(i);
        if (grade.getStatus().equals("Pass")) {
            System.out.println(grade.details());
            hasPassedStudents = true;
        }
    }
    
    if (!hasPassedStudents) {
        System.out.println("There are no passed students in this course.");
    }
}
public void listFailedStudents() {
    boolean hasFailedStudents = false;
    System.out.println("Failed Students:");
    
    for (int i = 0; i < grades.size(); i++) {
        Grade grade = grades.get(i);
        if (grade.getStatus().equals("Fail")) {
            System.out.println(grade.details());
            hasFailedStudents = true;
        }
    }
    
    if (!hasFailedStudents) {
        System.out.println("There are no failed students in this course.");
    }
}

    public String details(){
        String courseDetails="Course Name: "+courseName+"\tCourse ID: "+courseId+"\t\tDepartment: "+department.getName()+"\t\tStudent Type: "+studentType;
        return courseDetails;
    }

    Course get(int i) {
        throw new UnsupportedOperationException("Not supported yet."); //To change body of generated methods, choose Tools | Templates.
    }
}


class Student{
    private  String name;
    private  String id;
    private  int age;
    private Department department; 

    public Student() {
    }
    
    

    public Student(String name, String id, int age, Department department){ 
        this.name=name;
        this.id=id;
        this.age=age;
        this.department=department;
    }

    public String getName(){
        return name;
    }

    public String getId(){
        return id;
    }

    public int getAge(){
        return age;
    }

    public Department getDepartment(){ 
        return department;
    }

    public String getStudentType(){
        return "Student";
    }

    public String details(){ 
        return "Name: " + name + "\tID: " + id + "\tAge: " + age + "\t\tDepartment: " + department.getName() + "\t\tStudent Type: " + getStudentType();
    }
}



class Graduate extends Student{

    public Graduate() {
    }
    
    
    public Graduate(String name, String id, int age, Department department){
        super(name, id, age, department);
    }

    @Override
    public String getStudentType(){
        return "Graduate";
    }

    @Override
    public String details(){
        return super.details();
    }
}



class Undergraduate extends Student{

    public Undergraduate() {
    }
    
    
    
    public Undergraduate(String name, String id, int age, Department department){ 
        super(name, id, age, department);
    }

    @Override
    public String getStudentType(){
        return "Undergraduate";
    }

    @Override
    public String details(){
        return super.details();
    }
}





class Department{

    public Department() {
    }
    
    
    
    private String name;
    private ArrayList<Course> courses;

    public Department(String name){
        this.name=name;
        this.courses=new ArrayList<>();
    }

    public String getName(){
        return name;
    }

    public void addCourse(Course course){
        courses.add(course);
    }

    public void removeCourse(Course course){
        courses.remove(course);
    }

    public ArrayList<Course> getCourses(){
        return courses;
    }

    public void listCourses(){
        courses.forEach((course) -> {
            System.out.println(course.details());
        });
    }

    public String details(){
        String departmentDetails="Department: "+name;
        return departmentDetails;
    }
}



class Grade{
    private  Student student;
    private  Course course;
    private  int grade;
    private String status;

    public Grade() {
    }
    
    

    public Grade(Student student, Course course, int grade){
        this.student=student;
        this.course=course;
        this.grade=grade;
        setStatus();
    }

    private void setStatus(){
        
        if(grade < 25){
            this.status="Fail";
        }else{
            this.status="Pass";
        }
    }

    public Student getStudent(){
        return student;
    }

    public Course getCourse(){
        return course;
    }

    public int getGrade(){
        return grade;
    }

    public String getStatus(){
        return status;
    }

    public String details(){
        return "Student: "+student.getName()+"\tCourse: "+course.getCourseName()+"\tGrade: "+grade+"\tStatus: "+status;
    }
}



class StudentManagementSystem{
    private  ArrayList<Student> students;
    private  ArrayList<Course> courses;
    private  ArrayList<Department> departments;
    private  Scanner scanner;

    public StudentManagementSystem(){
        students=new ArrayList<>();
        courses=new ArrayList<>();
        departments=new ArrayList<>();
        scanner=new Scanner(System.in);
    }
    
    

    public void addStudent(){
        System.out.println("Enter Student Type\n1. Undergraduate\n2. Graduate");
        System.out.print("-->");
        int type=scanner.nextInt();
        if((type==1)||(type==2)){
            scanner.nextLine();
            System.out.print("Enter Student Name: ");
            String name=scanner.nextLine();
            System.out.print("Enter Student Age: ");
            int age=scanner.nextInt();
            scanner.nextLine(); 
            System.out.print("Enter Student ID: ");
            String id=scanner.nextLine();
            System.out.print("Enter Department Name: ");
            String deptName=scanner.nextLine();
            Department department=findDepartmentByName(deptName);
            if(department==null){
                System.out.println("This department is not added. Please try again.");
            }else{
            Student student;
            if(type==1){
                student=new Undergraduate(name, id, age, department);
            }else{
                student=new Graduate(name, id, age, department);
            }
            students.add(student);
            System.out.println("Student added successfully.");
        }
        }else{
            System.out.println("Invalid option! Please try again.");
        }
    }

    public void removeStudent(){
        System.out.print("Enter Student ID to remove: ");
        String id=scanner.nextLine();
        boolean found=false;

        for(int i=0; i<students.size(); i++){
            if(students.get(i).getId().equals(id)){
                students.remove(i);
                System.out.println("Student removed successfully.");
                found=true;
                break;
            }
        }

        if(!found){
            System.out.println("Student with ID "+id+" not found.");
        }
    }

     public void addCourse(){
        System.out.println("Enter Student Type\n1. Undergraduate\n2. Graduate");
        System.out.print("-->");
        int type=scanner.nextInt();
            scanner.nextLine();
            String studentType;
        switch (type) {
            case 1:
                studentType="Undergraduate";
                break;
            case 2:
                studentType="Graduate";
                break; 
            default:
                System.out.println("Invalid option! Please try again.");
                return;
        }

        System.out.print("Enter Course Name: ");
        String name=scanner.nextLine();
        System.out.print("Enter Course ID: ");
        String id=scanner.nextLine();
        System.out.print("Enter Department Name: ");
        String deptName=scanner.nextLine();
        Department department=findDepartmentByName(deptName);

        if(department==null){
            System.out.println("This department is not added. Please try again.");
            return; 
        }        
        
        Course course=new Course(name, id, department, studentType);
        courses.add(course);
        department.addCourse(course);
        System.out.println("Course added successfully.");
    }

     

    public void removeCourse(){
        System.out.print("Enter Course ID to remove: ");
        String id=scanner.nextLine();
        boolean found=false;

        for(int i=0; i<courses.size(); i++){
            if(courses.get(i).getCourseId().equals(id)){
                Course course=courses.get(i);
                Department department=course.getDepartment();
                department.removeCourse(course);
                courses.remove(i);
                System.out.println("Course removed successfully.");
                found=true;
                break;
            }
        }

        if(!found){
            System.out.println("Course with ID "+id+" not found.");
        }
    }
    
     public void addDepartment(){
        System.out.print("Enter Department Name: ");
        String deptName=scanner.nextLine();
        Department department=findDepartmentByName(deptName);

        if(department==null){
            department=new Department(deptName);
            departments.add(department);
            System.out.println("Department added successfully.");
        }else{
            System.out.println("This department already exists.");
        }
    }

    public void removeDepartment(){
        System.out.print("Enter Department Name to remove: ");
        String name=scanner.nextLine();
        boolean found=false;
        for(int i=0; i<departments.size(); i++){
            if(departments.get(i).getName().equalsIgnoreCase(name)){
                Department department=departments.get(i);
                department.getCourses().forEach((course) -> {
                    courses.remove(course);
                });
                departments.remove(i);
                System.out.println("Department removed successfully.");
                found=true;
                break;
            }
        }
        if(!found){
            System.out.println("Department with name "+name+" not found.");
        }
    }    

    public void enrollStudentInCourse(){
        System.out.print("Enter Course ID: ");
        String courseId=scanner.nextLine();
        System.out.print("Enter Student ID: ");
        String studentId=scanner.nextLine();

        Course course=findCourseById(courseId);
        Student student=findStudentById(studentId);

        if((course!=null)&&(student!=null)){
            course.enrollStudent(student);
        }else{
            System.out.println("Course or Student not found.");
        }
    }
    
     public void assignGradeToStudent(){
        System.out.print("Enter Course ID: ");
        String courseId=scanner.nextLine();
        System.out.print("Enter Student ID: ");
        String studentId=scanner.nextLine();
        System.out.print("Enter Grade (out of 50): ");
        int grade=scanner.nextInt();
        scanner.nextLine(); 

        if((grade<0)||(grade>50)){
            System.out.println("Invalid grade! Please enter a grade between 0 and 50.");
            return;
        }

        Course course=findCourseById(courseId);
        Student student=findStudentById(studentId);

        if((course!=null)&&(student!=null)){
            course.assignGrade(student, grade);
        } else {
            System.out.println("Course or Student not found.");
        }
    }

    private Course findCourseById(String courseId) {
    if (courseId == null) {
        return null; // or throw IllegalArgumentException if preferred
    }
    
    for (int i = 0; i < courses.size(); i++) {
        Course course = courses.get(i);
        if (course.getCourseId().equals(courseId)) {
            return course;
        }
    }
    
    return null; 
}
    private Student findStudentById(String studentId){
        for(Student student:students){
            if(student.getId().equals(studentId)){
                return student;
            }
        }
        return null;
    }
    
   private Department findDepartmentByName(String name) {
    if (name == null) {
        return null; // or throw IllegalArgumentException if preferred
    }
    
    for (int i = 0; i < departments.size(); i++) {
        Department dept = departments.get(i);
        if (dept.getName().equalsIgnoreCase(name)) {
            return dept;
        }
      
    }
        return null;
   }
   public void listCourses() {
    if (courses.isEmpty()) {
        System.out.println("There are no courses added yet.");
    } else {
        for (int i = 0; i < courses.size(); i++) {
            Course course;
            course = courses.get(i);
            System.out.println(course.details());
        }
    }
}
public void listStudents() {
    if (students.isEmpty()) {
        System.out.println("There are no students added yet.");
    } else {
        for (int i = 0; i < students.size(); i++) {
            Student student = students.get(i);
            System.out.println(student.details());
        }
    }
}
   public void listDepartments() {
    if (departments.isEmpty()) {
        System.out.println("There are no departments added yet.");
    } else {
        for (int i = 0; i < departments.size(); i++) {
            Department department = departments.get(i);
            System.out.println(department.details());
        }
    }
}
    
    public void listPassedStudentsInCourse(){
    System.out.print("Enter Course ID: ");
    String courseId=scanner.nextLine();

    Course course=findCourseById(courseId);

    if(course!=null){
        course.listPassedStudents();
    }else{
        System.out.println("Course not found.");
    }
}

public void listFailedStudentsInCourse(){
    System.out.print("Enter Course ID: ");
    String courseId=scanner.nextLine();

    Course course=findCourseById(courseId);

    if(course!=null){
        course.listFailedStudents();
    }else{
        System.out.println("Course not found.");
    }
}   
}
public class NewCode {


                      
     
    public static void main(String[] args) {
       
        Scanner scanner=new Scanner(System.in);

        System.out.println("*****************************************************************");
        System.out.println("|\t\tWELCOME TO STUDENT MANAGEMENT SYSTEM\t\t|");
        System.out.println("*****************************************************************");        
        
        try{
            boolean r=true;
            int login;
             StudentManagementSystem s=new StudentManagementSystem();
            while(r){
                System.out.println("");
                System.out.println("*************************");
                System.out.println("|   Login as:\t\t|");
                System.out.println("|\t1) Admin\t| \n|\t2) User\t\t| \n|\t3) Exit\t\t|");
                System.out.println("*************************");
                System.out.print("Choose an option: ");
                login=scanner.nextInt();
               
                switch(login){
                    case 1:
                        boolean password=false;
                        while(!password){
                            System.out.print("Enter Password: ");
                            int p=scanner.nextInt();
                            if(p==5436){
                                password=true;
                                boolean running=true;
                                while(running){
                                    System.out.println("");
                                    System.out.println("*************************************************");
                                    System.out.println("|\tYou want to:\t\t\t\t|");
                                    System.out.println("|\t1. Add Department\t\t\t|");
                                    System.out.println("|\t2. Add Student\t\t\t\t|");
                                    System.out.println("|\t3. Add Course\t\t\t\t|");
                                    System.out.println("|\t4. Remove Department\t\t\t|");
                                    System.out.println("|\t5. Remove Student\t\t\t|");
                                    System.out.println("|\t6. Remove Course\t\t\t|");
                                    System.out.println("|\t7. List Departments\t\t\t|");
                                    System.out.println("|\t8. List Students\t\t\t|");
                                    System.out.println("|\t9. List Courses\t\t\t\t|");
                                    System.out.println("|\t10. Enroll Student in Course\t\t|");
                                    System.out.println("|\t11. Assign Grade to Student\t\t|");
                                    System.out.println("|\t12. List Passed Students in Course\t|");
                                    System.out.println("|\t13. List Failed Students in Course\t|");
                                    System.out.println("|\t14. Exit\t\t\t\t|");
                                    System.out.println("*************************************************");
                                    System.out.print("Choose an option: ");
                                    int option=scanner.nextInt();
                                    scanner.nextLine();
                                    
                                    switch(option){
                                        case 1:
                                            s.addDepartment();
                                            System.out.println("");
                                            break;
                                        case 2:
                                            s.addStudent();
                                            System.out.println("");
                                            break;
                                        case 3:
                                            s.addCourse();
                                            System.out.println("");
                                            break;
                                        case 4:
                                            s.removeDepartment();
                                            System.out.println("");
                                            break;
                                        case 5:
                                            s.removeStudent();
                                            System.out.println("");
                                            break;
                                        case 6:
                                            s.removeCourse();
                                            System.out.println("");
                                            break;
                                        case 7:
                                            s.listDepartments();
                                            System.out.println("");
                                            break;
                                        case 8:
                                            s.listStudents();
                                            System.out.println("");
                                            break;
                                        case 9:
                                            s.listCourses();
                                            System.out.println("");
                                            break;
                                        case 10:
                                            s.enrollStudentInCourse();
                                            System.out.println("");
                                            break;
                                        case 11:
                                            s.assignGradeToStudent();
                                            System.out.println("");
                                            break;
                                        case 12:
                                            s.listPassedStudentsInCourse();
                                            System.out.println("");
                                            break;
                                        case 13:
                                            s.listFailedStudentsInCourse();
                                            System.out.println("");
                                            break;
                                        case 14:
                                            running=false;
                                            break;
                                        default:
                                            System.out.println("Invalid option! Please try again.");
                                            System.out.println("");
                                    }
                                }
                            }else{
                                System.out.println("Incorrect Password! Please try again.");
                            }
                        }
                        break;
                    case 2:
                        boolean uPass=false;
                        while(!uPass){
                            System.out.print("Enter Password: ");
                            int up=scanner.nextInt();
                            if(up==1234){
                                uPass=true;
                                boolean run=true;
                                while(run){
                                    System.out.println("");
                                    System.out.println("*************************************************");
                                    System.out.println("|\tYou want to:\t\t\t\t|");
                                    System.out.println("|\t1. List Departments\t\t\t|");
                                    System.out.println("|\t2. List Students\t\t\t|");
                                    System.out.println("|\t3. List Courses\t\t\t\t|");
                                    System.out.println("|\t4. List Passed Students in Course\t|");
                                    System.out.println("|\t5. List Failed Students in Course\t|");
                                    System.out.println("|\t6. Exit\t\t\t\t\t|");
                                    System.out.println("*************************************************");
                                    System.out.print("Choose an option: ");
                                    int option=scanner.nextInt();
                                    scanner.nextLine();
                                    
                                    switch(option){
                                        case 1:
                                            s.listDepartments();
                                            System.out.println("");
                                            break;
                                        case 2:
                                            s.listStudents();
                                            System.out.println("");
                                            break;
                                        case 3:
                                            s.listCourses();
                                            System.out.println("");
                                            break;
                                        case 4:
                                            s.listPassedStudentsInCourse();
                                            System.out.println("");
                                            break;
                                        case 5:
                                            s.listFailedStudentsInCourse();
                                            System.out.println("");
                                            break;
                                        case 6:
                                            run=false;
                                            break;
                                        default:
                                            System.out.println("Invalid option! Please try again.");
                                    }
                                }
                            }else{
                                System.out.println("Incorrect Password! Please try again.");
                            }
                        }
                        break;
                    case 3:
                        r=false;
                        System.out.println("*************************");
                        break;
                    default:
                        System.out.println("Invalid option! Please try again.");
                        break;
                }
            }
        }catch(Exception e){
            System.out.println(e);
        }
    }
}

    
    
