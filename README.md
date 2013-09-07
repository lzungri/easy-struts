easy-struts
===========

MVC web framework based on struts 1.2.9 that provides an easy declarative way for managing the navigation and parameters passing between web pages (named 'use cases' in easy-struts terminology)

It allows you to do the following:

```java
public class CUUnoUseCase extends AdminBaseUseCase {

  public Class useCaseModelClass() {
    return CUUnoUseCaseModel.class;
  }

  public String getShortDescription() {
    return "Use case one";
  }

  public String getLongDescription() {
    return "This is a long description for use case one";
  }

}

public class CUUnoUseCaseModel extends AdminBaseUseCaseModel{

  protected Class<? extends ModelObject> entityClass() {
    return Information.class;
  }	

  // Automatically invoked from the jsp after user interaction
  public void goToUCOne(UseCaseContext context) {
    context.goToChildUseCase(CUUnoUseCase.class, new SelectionMode(), "returnFromSelection");
  }

  public void returnFromSelection(UseCaseContext context) {
    CUUnoUseCaseModel model = (CUUnoUseCaseModel) context.getReturnedModel();
    if(model != null) {
      for(ModelObject selectedEntity : model.getSelectedEntities()) {
        context.addMessage(((Information) selectedEntity).getContent());
      }
    }
  }

  // Automatically invoked from the jsp after user interaction
  public void goToUCTwo(UseCaseContext context) {
    Map parametros = CollectionFactory.createMap();
    parametros.put("valor11", this.valor11);
    parametros.put("valor12", this.valor12);
  
    context.goToChildUseCase(CUDosUseCase.class, new EditMode(), parametros, "returnFromUCTwo");
  }

  public void returnFromUCTwo(UseCaseContext context) {
    context.addElement("returnedFrom", "Use case 2");
  }

}
```
