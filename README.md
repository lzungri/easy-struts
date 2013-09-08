easy-struts
===========

MVC web framework based on struts 1.2.9 that provides an easy declarative way for managing the navigation and parameters passing between web pages (named 'use cases' in easy-struts terminology)

It allows you to do the following:

```java

public class FirstUseCase extends AdminBaseUseCase {

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

  // Automatically invoked from the jsp after user interaction
  public void goToSelectUsersUseCase(UseCaseContext context) {
    Map parameters = CollectionFactory.createMap();
    parameters.put("userFilter1", "value1");
    parameters.put("userFilter2", "value2");

    context.goToChildUseCase(CRUDUserUseCase.class, new SelectionMode(), "returnFromSelection");
  }

  public void returnFromSelection(UseCaseContext context) {
    CRUDUserUseCaseModel model = (CRUDUserUseCaseModel) context.getReturnedModel();
    
    for(ModelObject selectedEntity : model.getSelectedEntities()) {
      User user = (User) selectedEntity;
      context.addMessage(user.getUsername());
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

  protected Predicate searchFilter(final Usuario loggedUser) {
    return new Predicate() {
      public boolean evaluate(Object object) {
        InformeEvaluacionFase informe = (InformeEvaluacionFase) object;
      return informe.getGrupoEvaluado().equals(loggedUser) && informe.getEstado().getDomainCode().equals(InformeEvaluacionFase.ESTADO_CODE_ENVIADA);
    }
  }

}

```

See [sisopadmin](https://github.com/lzungri/sisopadmin) project for a real-world example of easy-struts.

