/** question 1:Explain the relationship between the "Product" and "Product_Category" entities from the above diagram.**/

ques1>answer first question: ->one to one relationship is there in given example:
-- Create the Product table
CREATE TABLE Product (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255),
    description TEXT,
    SKU VARCHAR(50),
    category_id INT,
    inventory_id INT,
    price DECIMAL(10, 2),
    discount_id INT,
   created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    modified_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    deleted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    FOREIGN KEY (category_id) REFERENCES Product_Category(id),
    FOREIGN KEY (inventory_id) REFERENCES Inventory(id),
    FOREIGN KEY (discount_id) REFERENCES Discount(id)
    
);

-- Create the Product_Category table
CREATE TABLE Product_Category (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255),
    description TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    modified_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
    deleted_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP
);



/**2nd question:How could you ensure that each product in the "Product" table has a valid category assigned to it?**/
To ensure that each product in the Product table has a valid category assigned to it, you can add a foreign key constraint with the NOT NULL option on the category_id column. 
This will ensure that every product must have a valid category assigned to it upon insertion. Here's the modified table creation SQL:
CREATE TABLE Product (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(255),
    description TEXT,
    SKU VARCHAR(50),
    category_id INT NOT NULL, -- Ensures category_id cannot be NULL
    inventory_id INT,
    price DECIMAL(10, 2),
    discount_id INT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    modified_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    deleted_at TIMESTAMP DEFAULT NULL,
    FOREIGN KEY (category_id) REFERENCES Product_Category(id),
    FOREIGN KEY (inventory_id) REFERENCES Inventory(id),
    FOREIGN KEY (discount_id) REFERENCES Discount(id)
);

With category_id INT NOT NULL, you're ensuring that every product must have a valid category assigned to it, and it cannot be left as NULL during insertion. 
This ensures data integrity by guaranteeing that each product has a valid category.


/**3rd question:Create schema in any Database script or any ORM (Object Relational Mapping)**/
ans:one to many relationship used to create orm object in java using hibernate.
/*1)first file name : QuestiontoOne.java*/


package onetoMany;

import java.util.List;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.OneToMany;

@Entity
public class QuestiontoOne {
	@Id
	private int questionId;
	private String question;
	@OneToMany
	private List<Answermany> answers;
	public int getQuestionId() {
		return questionId;
	}
	public void setQuestionId(int questionId) {
		this.questionId = questionId;
	}
	public String getQuestion() {
		return question;
	}
	public void setQuestion(String question) {
		this.question = question;
	}
	public List<Answermany> getAnswers() {
		return answers;
	}
	public void setAnswers(List<Answermany> answers) {
		this.answers = answers;
	}
	
	
}


/*2nd file name:Answermany.java*/


package onetoMany;

import javax.persistence.Entity;
import javax.persistence.Id;
import javax.persistence.ManyToOne;

@Entity
public class Answermany {
	@Id
	private int answerId;
	private String answers;
	@ManyToOne
	private QuestiontoOne question;
	public int getAnswerId() {
		return answerId;
	}
	public void setAnswerId(int answerId) {
		this.answerId = answerId;
	}
	public String getAnswers() {
		return answers;
	}
	public void setAnswers(String answers) {
		this.answers = answers;
	}
	public QuestiontoOne getQuestion() {
		return question;
	}
	public void setQuestion(QuestiontoOne question) {
		this.question = question;
	}
	

}

/*3rd file name: Mapdem.java**/
package onetoMany;

import java.util.ArrayList;
import java.util.List;

import org.hibernate.Session;
import org.hibernate.SessionFactory;
import org.hibernate.Transaction;
import org.hibernate.cfg.Configuration;

public class Mapdemo {
	public static void main(String[] args) {
		SessionFactory f=new Configuration().configure("hibernateonetomany.cfg.xml").buildSessionFactory();
		Session session=f.openSession();
		Transaction t=session.beginTransaction();
		QuestiontoOne q=new QuestiontoOne();
		q.setQuestionId(002);
		q.setQuestion("what is java");
		Answermany a1=new Answermany();
		a1.setAnswerId(103);
		a1.setAnswers("java is a programming language");
		a1.setQuestion(q);
		Answermany a2=new Answermany();
		a2.setAnswerId(104);
		a2.setAnswers("with the help of java we can create software");
		a2.setQuestion(q);
		Answermany a3=new Answermany();
		a3.setAnswerId(105);
		a3.setAnswers("java has different types of framesworks");
		a3.setQuestion(q);
		List<Answermany> list=new ArrayList<Answermany>();
		list.add(a1);
		list.add(a2);
		list.add(a3);
		q.setAnswers(list);
		session.save(q);
		session.save(a1);
		session.save(a2);
		session.save(a3);
		t.commit();
		
		
		
	}

}

/*4th file name:*"hibernateonetomany.cfg.xml(orm file)*/
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
<hibernate-configuration>
	<session-factory>
		<property name="connection.driver_class">com.mysql.cj.jdbc.Driver</property>
		<property name="connection.url">jdbc:mysql://localhost/hibernateprog</property>
		<property name="connection.username">root</property>
		<property name="connection.password">Niharika2001</property>
		<property name="dialect">org.hibernate.dialect.MySQL8Dialect</property>
		<property name="connection.pool_size">3</property>
		<property name="current_session_context_class">thread</property>
		<property name="show_sql">true</property>
		<property name="format_sql">true</property>
		<property name="hbm2ddl.auto">update</property>

		<mapping class="onetoMany.QuestiontoOne" />
		<mapping class="onetoMany.Answermany"></mapping>

	</session-factory>


</hibernate-configuration>






