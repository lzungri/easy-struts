easy-struts
===========

MVC web framework based on struts 1.2.9 that provides an easy declarative way for managing the navigation and parameters passing between web pages (named 'use cases' in easy-struts terminology)

It allows you to do the following:

```java

public class FirstUseCase extends BaseUseCase {

  public Class useCaseModelClass() {
    return FirstUseCaseModel.class;
  }

  public String getShortDescription() {
    return "First Use case";
  }

  public String getLongDescription() {
    return "This is a long description for the first use case";
  }
  
  public boolean isVisibleOnMenu() {
    return true;
  }

}

public class FirstUseCaseModel extends BaseUseCaseModel{

  // Automatically invoked when the user clicks on the 'Select users' button
  public void goToSelectUsersUseCase(UseCaseContext context) {
    Map parameters = CollectionFactory.createMap();
    Calendar calendar = Calendar.getInstance();
    calendar.add(Calendar.DAY_OF_MONTH, -10);
    parameters.put("userCreationDateBefore", calendar.getTime());

    context.addMessage("Please select a user from the following list:");
    context.goToChildUseCase(CRUDUserUseCase.class, new SelectionMode(), "returnFromSelection");
  }

  public void returnFromSelection(UseCaseContext context) {
    CRUDUserUseCaseModel model = (CRUDUserUseCaseModel) context.getReturnedModel();
    
    for(User selectedUser : model.getSelectedEntities()) {
      context.addMessage("Selected user: " + selectedUse.getUsername());
    }
  }

}


public class CRUDUserUseCase extends AdminBaseUseCase {

  public Class useCaseModelClass() {
    return CRUDUserUseCaseModel.class;
  }

  public String getShortDescription() {
    return "Create, read, update, delete users use case";
  }

  public boolean isVisibleOnMenu() {
    return true;
  }

}

public class CRUDUserUseCaseModel extends ABMBaseUseCaseModel {

  protected Class<? extends ModelObject> entityClass() {
    return User.class;
  }	

  protected Predicate searchFilter(final User loggedUser) {
    return new Predicate() {
      public boolean evaluate(Object object) {
        User user = (User) object;
        Date date = context.getParameter("userCreationDateBefore");
        return user.getFechaAlta().before(date);
      }
    }
  }

}

```

See [sisopadmin](https://github.com/lzungri/sisopadmin) project for a real-world example of easy-struts.

